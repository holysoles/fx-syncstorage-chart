# fx-syncstorage
Helm chart for [Mozilla's Rust based Sync server](https://github.com/mozilla-services/syncstorage-rs/) for Firefox data synchronization.

# Usage

```shell
# add the chart repo
helm repo add fx-syncstorage https://holysoles.github.io/fx-syncstorage-chart/

# Customize the values file. Particularly make sure you update syncstorage.domain!
helm show values fx-syncstorage/fx-syncstorage > values.yaml

# install the chart
helm install --namespace fx-syncstorage --create-namespace fx-syncstorage/fx-syncstorage --values values.yaml
```

By default, this chart deploys 2 mysql instances with persistence for the Syncserver Tokenserver DBs. Credentials are automatically configured and stored in secrets.

If you'd like to try this chart out, you might want to run it without any persistence:

```shell
helm install fx-syncstorage holysoles/fx-syncstorage --set syncserverdb.primary.persistence.enabled=false --set tokenserverdb.primary.persistence.enabled=false --set syncstorage.domain=https://sync.example.com
```

# Notes

This chart currently uses the `0.13.6` version tag of the the `mozilla/syncstorage-rs` Docker image. However this is not the latest version. [There is an issue open tracking the problem](https://github.com/mozilla-services/syncstorage-rs/issues/1511), which prevents a database connection to non-Spanner DBs. If you decide to use a custom DB URL that connects to a Spanner instance, then you can probably use the latest version.

This chart defaults to deploying mysql instances with automatically configured credentials for the 2 required databases (though there was a discussion somewhere about if 2 separate databases are _truly_ necessary..). This helps reduce the amount of values you need to customize in your values file. To perform that automatic credential provisioning, due to limitations of Helm's `lookup` function, a service account is provisioned with the ability to _create_ secrets in the namespace where you deploy this chart to create the DB connection URLs secret during install, so it is recommended to deploy this chart to its own namespace.

The `fxaMetricsHashSecret` value is likely not necessary for this chart to maintain, as this is meant to obscure personal information when reported to Sentry, the metrics analytics tool used by Mozilla, but that reporting isn't meant to occur on self-hosted instances. If you are concerned about this possibility anyway, [check this blog post](https://www.kyzer.me.uk/syncserver/) out for how to patch that code out, and you can bring your own Docker image.

# Licensing

[MIT license](https://github.com/holysoles/fx-syncstorage-chart/blob/main/LICENSE).

The Mozilla syncstorage-rs project uses the [Mozilla Public License v2.0](https://github.com/mozilla-services/syncstorage-rs/blob/master/LICENSE).

Please refer to any bundled charts for their respective licenses.

# Resources and Credits

- [Mozilla's repo for the syncstorage-rs project](https://github.com/mozilla-services/syncstorage-rs)
- @kyz [wrote a guide for manually deploying an instance](https://www.kyzer.me.uk/syncserver/) which provides good insights
- @jeena has a [Docker compose example](https://github.com/jeena/fxsync-docker)
- Some other, older guides:
  - https://artemis.sh/2023/03/27/firefox-syncstorage-rs.html
  - https://blog.diego.dev/posts/firefox-sync-server/
  - https://aur.archlinux.org/packages/firefox-syncstorage-git#comment-840519
- [Great documentation here](https://github.com/SynoCommunity/spksrc/wiki/Mozilla-Sync-Server) by SynoCommunity on how to configure clients with a custom server.
