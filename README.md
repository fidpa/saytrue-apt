# Saytrue APT Repository

APT repository for the Linux packages of [Saytrue](https://saytrue.de) (amd64 and arm64), served at
`https://fidpa.github.io/saytrue-apt`. The `.deb` from [saytrue-releases](https://github.com/fidpa/saytrue-releases/releases)
registers this source on installation. To add it by hand:

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

The workflow `update-apt.yml` imports the `.deb` of a release with reprepro and deploys the repository to GitHub Pages.
It runs on `repository_dispatch` from the Linux build workflows or by hand.
