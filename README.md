# nem-docker

## Usage

### Build

```shell
$ docker-compose build
```

### Config

```text
./config
  └── nis-client
    └── config-user.properties
```

You can put user configuration file. Write your lines what you want to overwrite.

**Please don't overwrite `nem.folder` property.**
The path is set `/app/nis-client/data` by default for docker container environment.

### Data

```text
./data
  └── nis-client
    └── nis
      └── data
        └── nis5_mainnet.mv.db
```

### Run

```shell
# Docker desktop for Windows / Mac OSX
$ docker-compose up

# Docker for Linux (run current user)
$ USERID=$(id -u) GROUPID=$(id -g) docker-compose up
```

- [http://localhost:7890](http://localhost:7890)
