# fx-syncstorage
Helm chart for [Mozilla's Rust based Sync server](https://github.com/mozilla-services/syncstorage-rs/) for Firefox data synchronization.

# Usage

By default, this chart deploys 2 mysql instances with persistence for the Syncserver Tokenserver DBs. Credentials are automatically configured and stored in secrets.

If you'd like to try this chart out, you might want to run it without any persistence:

```shell
helm install fx-syncstorage holysoles/fx-syncstorage --set syncserverdb.primary.persistence.enabled=false --set tokenserverdb.primary.persistence.enabled=false --set syncstorage.domain=https://sync.example.com
```

# Licensing

This repo is licensed under the permissive MIT license. The Mozilla syncstorage-rs project uses the Mozilla Public License v2.0. Please refer to any bundled dependencies for their respective licenses.