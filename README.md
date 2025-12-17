# Go rock

This repository contains the SDK [rock](https://documentation.ubuntu.com/server/explanation/virtualisation/about-rock-images/) definitions for the [go](https://go.dev/) programming language.

This contains a minimal Go 1.22 toolchain which can be used to build a wide variety of Go applications. It also includes a minimal GCC toolchain to build the Go applications that have C dependencies.

Any additional dependencies can be either mounted at runtime or installed
with the apt installation included in this rock.

## Example

Let's build [`Chisel`](https://github.com/canonical/chisel)!

First clone the repository and checkout a specific tag:

```bash
git clone --depth 1 -b v1.1.0 https://github.com/canonical/chisel
cd chisel
```

Now lets launch the go container with the code directory mounted:

```bash
$ docker run --name=go-container --rm -it -v ./:/work go:1.22
2025-12-16T17:26:16.476Z [pebble] {"type":"security","datetime":"2025-12-16T17:26:16Z","level":"WARN","event":"sys_startup:0","description":"Starting daemon","appid":"pebble"}
2025-12-16T17:26:16.476Z [pebble] Started daemon.
2025-12-16T17:26:16.477Z [pebble] POST /v1/services 127.239µs 400 (http+unix)
2025-12-16T17:26:16.477Z [pebble] Cannot start default services: no default services
```

The rock is running, but [`pebble`](https://github.com/canonical/pebble) - the container entrypoint does not have any entry point. This is fine! This is not a rock with a service. Its for building applications. Let's now log into the container and build the application. In a separate terminal run:

```bash
docker exec -it go-container bash
```

to get a shell, and compile:

```bash
cd /work && go build -o chisel ./cmd/chisel
```

Let's now log out of the container and try our binary:

```bash
$  ./chisel --help   
Chisel can slice a Linux distribution using a release database
and construct a new filesystem using the finely defined parts.

Usage: chisel <command> [<options>...]

Commands can be classified as follows:

   Basic: find, info, help, version
  Action: cut

For more information about a command, run 'chisel help <command>'.
For a short summary of all commands, run 'chisel help --all'.
```

There you *go*!

## Available versions

* [Go 1.22 (Ubuntu 24.04)](./go-rock/1.22-24.04/rockcraft.yaml)
