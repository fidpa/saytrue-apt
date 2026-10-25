# Saytrue APT Repository

APT repository for the Linux packages of [Saytrue](https://saytrue.de) (amd64 and arm64), served at
`https://fidpa.github.io/saytrue-apt`. From the release after 0.2.0, the `.deb` from
[saytrue-releases](https://github.com/fidpa/saytrue-releases/releases) registers this source on installation;
the packages of 0.1.0 and 0.2.0 do not. With the source registered, your system installs updates of Saytrue
together with its other updates; from the same release on, the app then no longer checks GitHub for updates itself. To add it by hand:

```bash
curl -fsSL https://fidpa.github.io/saytrue-apt/pubkey.asc \
  | sudo gpg --dearmor -o /usr/share/keyrings/saytrue-archive-keyring.gpg

sudo tee /etc/apt/sources.list.d/saytrue.sources >/dev/null <<'SRC'
Types: deb
URIs: https://fidpa.github.io/saytrue-apt
Suites: stable
Components: main
Architectures: amd64 arm64
Signed-By: /usr/share/keyrings/saytrue-archive-keyring.gpg
SRC

sudo apt update && sudo apt install saytrue
```

To stop updates through this source but keep Saytrue, delete `/etc/apt/sources.list.d/saytrue.sources`. The app then checks GitHub for updates itself again; installing such an update installs a new `.deb`,
which registers the source again. To stop both for good, delete the file and switch off the automatic update check in
the app settings.
`sudo apt purge saytrue` removes the source together with the app, `sudo apt remove saytrue` keeps it.

The workflow `update-apt.yml` imports the `.deb` of a published release with reprepro and deploys the repository to
GitHub Pages. The ARM64 Linux build triggers it for both architectures once the release is published; it can also
be run by hand.
