# Create a provider

This tutorial aims to help you create and modify your first provider. It will
not touch "advanced" cell types like the graph and the log cells, but will help
you integrate a third-party resource so that any output from it can be added to
a notebook with only a `/` command (we call those "slash commands").

This tutorial focuses on writing the provider in Rust, to be able to use the
convenience macros from Fiberplane Plugin Development Kit (PDK). Writing
providers in different languages will be addressed in a different guide.

## Prepare the relevant resources

### Install Rust and the Web Assembly tools

The necessary tools to install to work with Fiberplane Plugin Development Kit are:

#### Rustup

[Rustup](https://rustup.rs/), the Rust toolchain handler. Follow the tutorial
to install Rust on your machine.

#### WebAssembly compilation target

Add the wasm32-unknown-unknown "target" with `rustup target add wasm32-unknown-unknown`.

#### WebAssembly optimizers

Install wasm-opt to optimize your plugins before they reach production. Wasm-opt
is part of [binaryen tools](https://github.com/WebAssembly/binaryen). The best
way to install this is to

- Go to the [releases page](https://github.com/WebAssembly/binaryen/releases)
and then download the tarball for the latest release (version 111 as of the 
day of writing). I am showing it in command line:
```console
$ wget https://github.com/WebAssembly/binaryen/releases/download/version_111/binaryen-version_111-arm64-macos.tar.gz
<some output>
$ ls -la
.rw-r--r--@ 5.1M you 22 Nov  2022 binaryen-version_111-arm64-macos.tar.gz
```
- Decompress the tarball in a directory on your machine.
```console
$ tar -xzf binaryen-version_111-arm64-macos.tar.gz
$ ls binaryen-version_111/
bin  include  lib
```
- Add the `bin` subdirectory of the tarball to your path. In the example below,
we assume that the tarball has been decompressed in `/Users/you/opt`
```sh
# In your shell rc file (.bashrc, .zshrc, .config/fish/config.fish....)
export PATH="/Users/you/opt/binaryen-version_111/bin:${PATH}"
```

#### Checkpoint

Start a new terminal to make sure that all your environment variables are
properly "refreshed", and then make sure that you have all the correct tooling
for the rest of the tutorial:

```console
$ which cargo
/Users/you/.cargo/bin/cargo
```
If you don't have `cargo`, you probably missed something in the Rust
installation step in [Rustup](https://rustup.rs).

```console
$ rustup target list --installed
aarch64-apple-darwin
wasm32-unknown-unknown
```
The `aarch64-apple-darwin` is the native target of my machine (Apple Silicon on
MacOS), the important part is to make sure that you have
`wasm32-unknown-unknown` target. If you don't have it, add the target with
`rustup target add wasm32-unknown-unknown`

```console
$ which wasm-opt
/Users/you/opt/binaryen-version_111/bin/wasm-opt
```

### Find the documentation of the external resource

For this tutorial we will integrate TBD with Fiberplane. Therefore we need to
find the relevant documentation for it, and decide on the necessary
configuration values we want to have in the provider.

#### Deciding on the features we can have and want to have

#### Deciding on configuration schema for the provider

#### Testing a SDK for serverless WebAssembly compilation

Sometimes the third party resource you want to integrate provides a Rust SDK.
But beware! There are a few libraries that will fail (either at compile time or
at run time) when compiled for serverless WASM. So if you are not sure that the
SDK will work with serverless wasm, it is better to find a classic API (on top
of HTTP or WebSocket protocols) documentation to communicate with the third
party resource. We will cover "finding out if the SDK works in serverless wasm"
when setting up the project.

When things don't work as expected, you can ask for help or guidance online[^serverlesswasm].

[^serverlesswasm]: The space of serverless WebAssembly is still
    young and a lot of package maintainers can make the change or help you make the change to get a library to compile and
    run for WebAssembly. Sometimes it means finding out support is [already there](https://github.com/gyscos/zstd-rs/pull/139),
    sometimes it means [adding items to their roadmap](https://github.com/awslabs/aws-sdk-rust/discussions/679)

#### Checkpoint

At that point you must know
- what configuration values are necessary to communicate with the third party
  resource
- a documentation/reference to be able to programmatically communicate with said
  resources

### Clone the Providers repo

The current Fiberplane [providers repo](https://github.com/fiberplane/providers)
contains a "sample"
[provider](https://github.com/fiberplane/providers/tree/main/providers/sample)
that is a good base to make your provider from: our CI pipeline guarantees that
the sample provider is always up-to-date with our supporting libraries, so it is
the quickest way to obtain a starting template.

Clone the repository and keep the location of the sample provider somewhere handy:
```console
$ git clone https://github.com/fiberplane/providers
$ export SAMPLE_PROVIDER_PATH="$(pwd)/providers/providers/sample"
```

#### Checkpoint

If you never restart your terminal session again, we will have a valid path for
`${SAMPLE_PROVIDER_PATH}`. We will not use it for long, but the point is that
you should have a fresh copy of the Fiberplane/providers repository.



## Instantiate a Daemon and side-load your sample provider for use in Studio

### Create your new provider repo, and test sample compilation

This step acts as a checkpoint. No reader should go past this point unless we held their hand through:
- installing a complete rust + wasm toolchain
- prepared their repo for the provider (including a .cargo/config.toml to auto-set the toolchain)
- tested that they could compile + wasm-opt the sample provider.

> Checkpoint: make the user produce a `username_provider.wasm` blob that is acceptable for proxy

### Install Fiberplane Daemon and setup the token to load the sample provider in the proxy

> Checkpoint: make the user run the proxy and obtain valid log messages showing `username_provider.wasm` is loaded

### Test the Sample provider in Studio

> Checkpoint: make the user use the sample provider in their Studio workspace


## Modify the sample provider to implement _your_ integration

### Remove the unused sample calls

### Adding a new query

- Choosing a name for the query
- Updating the `supported_query_types` list
- Implementing the handler for `invoke`

> Checkpoint: make the user test the new command in their Studio workspace

### Adding a query that returns more than just cells

- Implementing the handler for `create_cells`
- Implementing the handler for `extract_data`
