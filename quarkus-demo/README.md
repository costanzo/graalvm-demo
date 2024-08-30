#

## build

build executable

```shell
mvn -Pnative clean package
```

Test API

```shell
$ curl http://localhost:8080/hello
Hello from Quarkus REST
```