# TDP Phoenix QueryServer Notes

The version 6.0.0-TDP-0.1.0-SNAPSHOT of Phoenix QueryServer is based on the `6.0.0` tag of the Apache [repository](https://github.com/apache/phoenix-queryserver/tree/6.0.0).


# Build a queryserver with TDP versions

```
mvn clean package -Dpackage.phoenix.client -Dphoenix.version=5.1.3-TDP-0.1.0-SNAPSHOT -Dphoenix.client.artifactid=phoenix-client-hbase-2.1 -pl '!phoenix-queryserver-it' -DskipTests
```


This command generates a `tar.gz` file of the release at `phoenix-queryserver-assembly/target/phoenix-queryserver-6.0.0-TDP-0.1.0-SNAPSHOT-bin.tar.gz`

## Testing parameters

```
mvn verify
```

Phoenix QueryServer doesn't support running integrations tests against non-default Phoenix and HBase versions.
