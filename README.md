# archlinux-keyring

The archlinux-keyring project holds PGP packet material and tooling
(`keyringctl`) to create the distribution keyring for Arch Linux.
The keyring is used by pacman to establish the web of trust for the packagers
of the distribution.

The PGP packets describing the main signing keys can be found below the
[keyring/main](keyring/main) directory, while those of the packagers are located below the
[keyring/packager](keyring/packager) directory.

## Requirements

The following packages need to be installed to be able to create a PGP keyring
from the provided data structure and to install it:

Build:

* make
* findutils
* pkgconf
* systemd

Runtime:

* python
* sequoia-sq >= 0.31.0

Optional:

* hopenpgp-tools (verify)
* git (ci)

## Usage

### Build

Build all PGP artifacts (keyring, ownertrust, revoked files) to the build directory
```bash
./keyringctl build
```

### Import

Import a new packager key by deriving the username from the filename.
```bash
./keyringctl import <username>.asc
```

Alternatively import a file or directory and override the username
```bash
./keyringctl import --name <username> <file_or_directory...>
```

Updates to existing keys will automatically derive the username from the known fingerprint.
```bash
./keyringctl import <file_or_directory...>
```

Main key imports support the same options plus a mandatory `--main`
```bash
./keyringctl import --main <username>.asc
```

### Export

Export the whole keyring including main and packager to stdout
```bash
./keyringctl export
```

Limit to specific certs using an output file
```bash
./keyringctl export <username_or_fingerprint_or_directory...> --output <filename>
```

### List

List all certificates in the keyring
```bash
./keyringctl list
```

Only show a specific main key
```bash
./keyringctl list --main <username_or_fingerprint...>
```

### Inspect

Inspect all certificates in the keyring
```bash
./keyringctl inspect
```

Only inspect a specific main key
```bash
./keyringctl inspect --main <username_or_fingerprint_or_directory...>
```

### Verify

Verify certificates against modern expectations and assumptions
```bash
./keyringctl verify <username_or_fingerprint_or_directory...>
```

## Installation

To install archlinux-keyring system-wide use the included `Makefile`:

```bash
make install
```

## Contribute

Read our [contributing guide](CONTRIBUTING.md) to learn more about guidelines and
how to provide fixes or improvements for the code base.

## Releases

[Releases of
archlinux-keyring](https://gitlab.archlinux.org/archlinux/archlinux-keyring/-/tags)
are exclusively created by [keyring maintainers](https://gitlab.archlinux.org/archlinux/archlinux-keyring/-/project_members?with_inherited_permissions=exclude).

The tags are signed with one of the following legitimate keys:

```
Christian Hesse <eworm@archlinux.org>
02FD 1C7A 934E 6145 4584  9F19 A623 4074 498E 9CEE

David Runge <dvzrv@archlinux.org>
991F 6E3F 0765 CF62 9588  8586 139B 09DA 5BF0 D338

Florian Pritz <bluewind@archlinux.org>
CFA6 AF15 E5C7 4149 FC1D  8C08 6D16 55C1 4CE1 C13E

Johannes Löthberg <demize@archlinux.org>
5134 EF9E AF65 F95B 6BB1  608E 50FB 9B27 3A9D 0BB5

Leonidas Spyropoulos <artafinde@archlinux.org>
B4B7 5962 5D46 3343 0B74  8770 59E4 3E10 6B24 7368

Levente Polyak <anthraxx@archlinux.org>
E240 B57E 2C46 30BA 768E  2F26 FC1B 547C 8D81 72C8

Morten Linderud <foxboron@archlinux.org>
C100 3466 7663 4E80 C940  FB9E 9C02 FF41 9FEC BE16
```

To verify a tag, first import the relevant PGP keys:

```bash
gpg --auto-key-locate wkd --search-keys <email-from-above>
```

Afterwards a tag can be verified from a clone of this repository. Please note
that one **must** check the used key of the signature against the legitimate
keys listed above:

```bash
git verify-tag <tag>
```

## License

Archlinux-keyring is licensed under the terms of the **GPL-3.0-or-later** (see
[LICENSE](LICENSE)).
