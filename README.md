# Chauffeur

![Current Version](https://img.shields.io/badge/version-v0.1-blue)
![Docker Pulls](https://img.shields.io/docker/pulls/ethdotlimo/chauffeur)
![Twitter Follow](https://img.shields.io/twitter/follow/eth_limo?style=social)

## Table of Contents
- [Getting Started](#getting-started)
	- [Tools Required](#tools-required)
	- [Local Configuration](#local-configuration)
	- [Service Configuration](#service-configuration)
	- [Startup](#startup)
- [Authors](#authors)
- [License](#license)

## Getting Started

### Tools Required

- [Docker Compose](https://docs.docker.com/compose/)

### Local Configuration

Chauffeur requires two operating-system-level configuration changes in order to function. The exact instructions for
these changes will vary by operating system type, distribution, and configuration, and involve injecting configuration
into system services which control how your computer resolves internet links and how it determines which SSL certificates
are valid.

As such, you are **strongly recommended** to understand these changes before implementing them, and are advised that
these changes may result in instability or insecurity if done incorrectly.

#### DNS Resolution

The first change to make is to configure resolution of the `eth.local` domain namespace.

##### systemd-resolved (e.g. RHEL, Fedora)

Create the file `/etc/systemd/resolved.conf.d/eth_limo.conf`, with the following content:

```
[Resolve]
DNS=172.28.0.1
Domains=~eth.local
```

Then, execute `systemctl restart systemd-resolved.service`.

#### Caddy SSL Root Certificate

After starting Chauffeur as indicated in the [Startup](#startup) section, a directory path will be created in the root
of this repository named `certificates/pki/authorities/chauffeur`. Within this directory will be a file named `root.crt`,
which is an [SSL Root Certificate](https://en.wikipedia.org/wiki/Root_certificate) from which individual certificates
will be generated for ENS subdomains (e.g. `vitalik.eth.local`).

This root certificate needs to be added to your local trust store in order for the certificates issued by Chauffeur
to be regarded as valid by your browser or other applications.

##### p11-kit (Linux)

Most Linux distributions have a utility simply named [`trust`](https://www.mankier.com/1/trust) available, which will
add a specified certificate to the system's trust policy store.

To do this, run `trust anchor certificates/pki/authorities/chauffeur/root.crt`.

### Service Configuration

- Optionally copy `.env.dist` to `.env`, and populate any of the specified environment variables according to your needs

### Startup

To start Chauffeur, simply run `docker-compose up` in the root of this repository.

Optionally, specify `--profile trustless` in order to launch the Geth light client service for RPC.

## Authors

#### Justin Martin
* [GitHub](https://github.com/TheFrozenFire/)
* [Twitter](https://twitter.com/thefrozenfire)

You can also see the complete [list of contributors][contributors] who participated in this project.

## License

`Chauffeur` is open source software [licensed as MIT][LICENSE].
