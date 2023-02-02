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

It's time to create your provider repo. To avoid the usual generic names, we
will create a `catnip` provider.

```console
$ mkdir catnip_provider
$ cd catnip_provider
$ cp -r "${SAMPLE_PROVIDER_PATH}"/* .
$ git init .
```

Once the repo is created, use the Cargo configuration feature to make the
compiler default to Web Assembly compilation. Create a `.cargo` folder at the
root of your repository, and a `config.toml` file[^profilerelease] inside:

```toml
# In .cargo/config.toml
[build]
target = "wasm32-unknown-unknown"
```

[^profilerelease]: You can also add a few other flags if you want. At Fiberplane
    we add a few flags for building the providers using the least disk-space
    possible, you can see them in our
    [repository](https://github.com/fiberplane/providers/blob/main/providers/.cargo/config)
    
The last step to setup your replica of the sample provider is to own it by
editing the `cargo.toml` file to set the name of the library:

```toml
# In Cargo.toml
name = "catnip_provider"
# ... Also edit the other metadata fields as you see fit.
```


#### Checkpoint

Try to compile your provider, make sure that it's targeting web assembly and
then optimize the given web assembly binary.

```console
## Change to the directory of your provider
$ cargo build && wasm-opt -Oz -c -o "./username.wasm" "target/wasm32-unknown-unknown/debug/catnip_provider.wasm"
```

**Compilation Error**

If you have a compilation error, and just copied the sample provider, make sure
that you pulled the lastest version of the sample provider:

```console
cd ${SAMPLE_PROVIDER_PATH}
git checkout main && git push
cd /path/to/catnip_provider
rm -rf .
mkdir -p .cargo && cp "${SAMPLE_PROVIDER_PATH}/../.cargo/config" .cargo/config.toml
cp -r "${SAMPLE_PROVIDER_PATH}"/* .
```

And try to compile again.
```console
$ cargo build
```

If it still fails, then it's an issue with our own provider development kit. Please file an issue
to the repo with the title "Sample Provider does not compile".

**Wasm optimization error**

If the `wasm-opt` operation fails, it can mostly fail for 2 reasons:
- `wasm-opt` is not in `PATH`. If so, check the setup step earlier and make sure
  to pass the checkpoint
- `target/wasm32-...` doesn't exist.
  + check that the name of the library crate matches the name of the wasm you're
    optimizing.
  + check that you have correctly created the `.cargo/config.toml` file that
    changes the default compilation target to `wasm32-unknown-unknown`.

### Install Fiberplane Daemon and setup the token to load the sample provider in the proxy

TODO

> Checkpoint: make the user run the proxy and obtain valid log messages showing `catnip_provider.wasm` is loaded

### Test the Sample provider in Studio

TODO

> Checkpoint: make the user use the sample provider in their Studio workspace


## Modify the sample provider to implement _your_ integration

In the following, we use "query" to describe one type of request that is handled
by the provider, including all the requests that make the slash commands in the
notebook later, but also the "built-in" requests used by Studio for extra
features such as health reporting and auto-suggestions.

### Implement your configuration type

### Adding a new query

Let's make a query that prompts the notebook users for a latitude/longitude, and
return information about the closest user from
`https://jsonplaceholder.typicode.com/` API geographically. This is a little
convoluted, but the point is to show multiple things when creating your
provider.

Let's assume that all users from `https://jsonplaceholder.typicode.com/` are
catnip dispensers and we want to know where to go to get our fix.

#### Choosing a name for the query

The query name be used in the URL to encode the request type for the provider. Let's use
`x-closest-dispenser`. The `x-` prefix is mandatory[^whyx].

[^whyx]: The `x-` prefix ensures that there will never be collisions with built-in query
    types used by Studio, like the queries Studio uses to query the status of a provider,
    or completion suggestions.

#### Updating the `supported_query_types` list

TODO

#### Implementing the handler for `invoke`

TODO

#### Checkpoint

> Checkpoint: make the user test the new command in their Studio workspace

### Implementing Status query

### Implementing Suggestions query

### Adding a query that returns more than just cells

- Implementing the handler for `create_cells`
- Implementing the handler for `extract_data`
