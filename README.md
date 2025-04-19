# nem-docker

## Usage

### Build

```shell
$ docker compose -f compose.yml build
```

### Config

```text
./config
  └── nis
    └── config-user.properties
```

You can put user configuration file. Write your lines what you want to overwrite.

**Please don't overwrite `nem.folder` property.**
The path is set `/data/nem` by default for docker container environment.

### Data

```text
./data
  └── nis
    └── data
      └── nis5_mainnet.mv.db
```

### Run

```shell
# Docker desktop for Windows / Mac OSX
$ docker compose -f compose.yml up

# Docker for Linux (run current user)
$ USERID=$(id -u) GROUPID=$(id -g) docker -f compose.yml compose up
```

- [http://localhost:7890](http://localhost:7890)
