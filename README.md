# fx-syncstorage
Helm chart for [Mozilla's Rust based Sync server](https://github.com/mozilla-services/syncstorage-rs/) for Firefox data synchronization.

# Usage

```shell
# add the chart repo
helm repo add fx-syncstorage https://github.com/holysoles/fx-syncstorage-chart

# Customize the values file. Particularly make sure you update syncstorage.domain!
helm show values fx-syncstorage/fx-syncstorage > values.yaml

# install the chart
helm install --namespace fx-syncstorage --create-namespace fx-sync/fx-syncstorage --values values.yaml
```

By default, this chart deploys 2 mysql instances with persistence for the Syncserver Tokenserver DBs. Credentials are automatically configured and stored in secrets.

If you'd like to try this chart out, you might want to run it without any persistence:

```shell
helm install fx-syncstorage holysoles/fx-syncstorage --set syncserverdb.primary.persistence.enabled=false --set tokenserverdb.primary.persistence.enabled=false --set syncstorage.domain=https://sync.example.com
```

# Licensing

[MIT license](https://github.com/holysoles/fx-syncstorage-chart/blob/main/LICENSE).

The Mozilla syncstorage-rs project uses the [Mozilla Public License v2.0](https://github.com/mozilla-services/syncstorage-rs/blob/master/LICENSE).

Please refer to any bundled charts for their respective licenses.