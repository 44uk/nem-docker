# nem-docker

## Usage

### Build

```shell
$ docker-compose build
```

### Config

Put files like this.

```text
data
├── nem
│   ├── config-user.properties
│   └── nis
│       └── data
│           └── nis5_mainnet.mv.db
└── servant
    └── config.properties
```

### Run

```shell
$ docker-compose up nis
```

- [http://localhost:7890](http://localhost:7890)
- [https://localhost:7891](https://localhost:7891)
