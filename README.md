<img src="http://silverlakecorp.com/torrentwatch-xa/torrentwatch-xa-logo144.png" width="144" height="144"/>

torrentwatch-xa
===============

__torrentwatch-xa is an anime/manga/light novel/TV show broadcatcher__ that regularly monitors multiple subscribed public RSS/Atom feeds for the latest "Favorite" serialized torrents and downloads them automatically. It is an actively-developed, high-quality resurrection of the popular but long-abandoned TorrentWatch-X.

As a fork of TorrentWatch-X, torrentwatch-xa handles Western live-action show titles containing commonly-used season x episode or date-based numbering styles. It is specially designed to __also__ handle the widely-varying numbering styles used by anime, manga, and light novel fansubbing crews and also features all the bugfixes and code cleanup that TorrentWatch-X so badly needed.

![torrentwatch-xa twxa ScreenShot 1](http://silverlakecorp.com/torrentwatch-xa/twxaScreenShot1.png)

To auto-download Favorite torrents, torrentwatch-xa controls a local __or remote__ Transmission BitTorrent client via Transmission RPC __and/or__ saves .torrent files or magnet links as files locally. The latter allows the use of __any__ BitTorrent client (not just Transmission) that can watch directories for .torrent files or magnet links to automatically start those torrents.

torrentwatch-xa runs on an Apache 2.4.x webserver with PHP 5.6.0alpha3&sup1; or higher and the prerequisite PHP packages listed in the installation instructions. It works out-of-the-box on any up-to-date instance of Debian 8.x, Ubuntu 14.04.x, or Ubuntu 16.04.x on any architecture, and it can be made to work on current versions of RedHat, Fedora, or CentOS LINUX by installing the RPM package equivalents of the prerequisite PHP .deb packages and adjusting the firewall and SELINUX restrictions. RedHat distros are not officially supported at this time.

torrentwatch-xa is extremely lightweight and can run decently on even a $5 Raspberry Pi Zero&sup2;. The web UI works on any modern web browser that has Javascript enabled, including smartphone and tablet browsers.

Common setups:

- __Local__: torrentwatch-xa and Transmission run together on the same LINUX desktop, server, or NAS; downloaded content is stored on this one device. The ODROID HC1 or HC2 with a large capacity SATA drive is perfect for this use case--quiet, fast, and easy to directly install torrentwatch-xa and transmission-daemon on.
- __Remote__: torrentwatch-xa runs on a low-power computer (usually a home-theater single-board computer running Kodi) or virtual machine and remotely controls Transmission running on a separate NAS that stores the downloaded content.


&sup1; PHP 5.6.0alpha3 is really only required by PHPMailer's SMTP 5.2.23 library to support TLS 1.1 and 1.2. torrentwatch-xa itself only requires PHP 5.4.0. If you are not using email triggers with TLS 1.1 or 1.2, you should be able to avoid this version requirement by downgrading PHPMailer's SMTP library.

&sup2; It takes around 18 seconds for the web UI to process all six default feeds with 32 favorites, as compared to around 5 seconds on an ODROID C1+.

Status
===============

__NOTE:__ In Ubuntu 18.04 (not yet supported), there is a new systemd security feature called PrivateTmp preventing Apache2 from writing to /tmp/twxalog; instead, a temporary, private, virtual /tmp folder is created for Apache2 to write to. Meanwhile, the cron job continues to write to /tmp/twxalog. This results in there being two twxalog files on the system. __PrivateTmp does not affect the normal function of torrentwatch-xa: it will still download items properly. However, if you don't want dual log files, you can opt to disable PrivateTmp for Apache2.__ In 1.2.0, the files placed in /tmp will be moved to other locations so that PrivateTmp does not have to be disabled.

I've posted 1.1.0 with the changes listed in [CHANGELOG.md](CHANGELOG.md).

__twxacli.php has been renamed to twxa_cli.php to conform to the file naming convention; please be aware that the cron job file will need to be updated to call twxa_cli.php__

1.1.0 fixed some leftover bugs in changing install paths by moving the hidden settings Cache Dir and History from torrentwatch-xa.config to twxa_config_lib.php.

A very bad but very rare bug where the Configure settings were wiped and replaced with some default settings was finally fixed in 1.1.0. The bug was carried over from TorrentWatch-X 0.8.9 but was made worse by the conversion of the config file to JSON. Fixing it required a major rewrite of readjSONConfigFile() to change the way torrentwatch-xa.config and torrentwatch-xa-config.cache are handled.

This config file bug had to be fixed in order to add a new, experimental feature: a command-line bulk Favorites importer, twxa_fav_import.php, now in alpha. You can now import a plain-text TSV (tab-separated values) file directly into your Favorites. Please read the usage notes for instructions.

The formerly hidden setting debugLevel was renamed to Log Level and exposed via Configure > Interface > Log Level

There's a new configuration option: Configure > Interface > Check for Updates

1.1.0 has improved checks for important files or directories that do not exist or have incorrect permissions.

#### Still in Alpha

- a Favorite Filter can now match multibyte strings (Japanese/Chinese/Korean) in RegEx matching mode only (not Simple, nor Glob), but multibyte characters must be individually specified in PCRE Unicode hexadecimal notation like `0x{3010}` to satisfy PHP's preg_ functions.
- Fedora Server 25 and Ubuntu 18.04 are being tested and work fine but will not be officially supported for quite a while.

#### Any Torrent Atom Feeds Out There?

I'd like to finally test and bugfix the Atom feed capability of torrentwatch-xa. If anyone knows of a public Atom feed that contains torrents, please message me at dchang0 at Github or open an Issue. If it's got anime torrents, even better, as I can include it as a default feed. Thanks!

Documentation
===============

See:

- [INSTALL.md](INSTALL.md) for detailed installation steps or important notes if you are upgrading from a prior version.

- [USAGE.md](USAGE.md) for usage notes and an explanation of some design decisions.

- [TROUBLESHOOTING.md](TROUBLESHOOTING.md) for detailed troubleshooting steps and explanations of design decisions and common issues.

- Known bugs are tracked primarily in the [TODO.md](TODO.md) and [CHANGELOG.md](CHANGELOG.md) files. Tickets in GitHub Issues will remain separate for accountability reasons.

Credits
===============

- Original TorrentWatch-X by Joris Vandalon https://code.google.com/p/torrentwatch-x/
- Original Torrentwatch by Erik Bernhardson https://code.google.com/p/torrentwatch/
- Credits for the few third-party PHP and Javascript libraries are inside of their respective files.