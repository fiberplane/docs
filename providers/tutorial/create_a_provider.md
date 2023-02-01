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

- Cover rustup
- Install the wasm32-unknown-unknown toolchain
- Install wasm-opt

> Checkpoint: make the user check for `which cargo`, `which wasm-opt`, and `rustup list-targets` or something

### Find the documentation of the external resource

- Best case a REST API
- Quickly explain how to test a SDK for wasm compilation
- Decide on the necessary dynamic information needed for configuration (URLs, credentials...)

### Clone the Providers repo

This step is _not_ included to contribute, it is just included so that users can copy-paste the sample provider


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
