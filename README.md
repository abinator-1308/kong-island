# kong-island
dev-test setup for kong, the cloud-native API gateway.

# Description
This setup helps to develop multiple kong plugins. It uses [konga](https://github.com/pantsel/konga) for dashboard.

# Features
1. Setup to develop multiple kong plugins locally.
2. Public kong plugins can be submoduled and used.
3. Pre-seeded `konga` configuration for local dashboard access.
4. Build deployable custom `kong` image with plugins configured.
5. Easy `make` targets for the functionalities defined.
6. `Dockerfile` can be used to build kong image with custom plugins.

# Directories
The repo has the following important directories:

- [kong-plugins](https://github.com/abinator-1308/kong-island-lean/tree/master/kong-plugins): This is a repository of all kong custom plugins. Some of them can be `submodule`d from other public git repositories.
- [kong-pongo](https://github.com/Kong/kong-pongo): Tooling to run plugin tests with Kong.

# Setup
Clone master branch  with submodules
```sh
git clone --recurse-submodules -j8 https://github.com/abinator-1308/kong-island-lean
cd kong-island
```

To fetch a specific branch
```sh
git clone https://github.com/abinator-1308/kong-island-lean
cd kong-island
git fetch origin <branch>
git submodule init
git submodule update
```

Initialize using make
```sh
make init # This will make pongo executable available in your host's path.
make help # This will print all available make targets.
down                           Brings down kong, cassandra and konga
help                           Shows help.
init                           Initialization: Symblinks kong-pongo's executable to host's path.
lint                           Runs linters for all kong plugins.
up                             Brings up kong, cassandra and konga
```

Export `KONG_VERSION` environment variable. The version is mentioned in `Makefile`
```
export KONG_VERSION=<version>
```

To change `KONG_VERSION`
1. Change `KONG_VERSION` in `Makefile`
2. Export the updated `KONG_VERSION` environment variable.
3. Update the kong image version in Dockerfile i.e `FROM kong:2.0.x`
 
Bring up kong, cassandra and konga
```sh
make up
# Access kong at http://127.0.0.1:8000/
# Access kong admin API at http://127.0.0.1:8001/
# Access konga at http://127.0.0.1:1337/ 
# Default user credentials for konga: root/root123
```

Bring down kong, cassandra and konga
```sh
make down
```

# Plugin development

All plugins are available under [kong-plugins dir](https://github.com/abinator-1308/kong-island-lean/tree/master/kong-plugins).

## Including an open-source plugin
Add the open-source plugin as a submodule in `kong-island`
```sh
git submodule add http://github.com/Kong/kong-plugin
```

## Enabling the plugin

Edit the `kong.conf` configuration file to make the following changes

1. Add the plugin in `plugins` as comma-separated values

```
plugins=myplugin
```

## Demo Video
[https://www.youtube.com/watch?v=YyRvzT6ng9U](https://www.youtube.com/watch?v=YyRvzT6ng9U)
