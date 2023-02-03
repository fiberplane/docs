---
title: Provider Protocol Reference
category: 63d7e8bdbf7b4b0e0745e823
slug: provider-protocol-reference
---

# Provider Protocol Reference (v2)

All functions decribed here take msgpack-serialized payloads as arguments and
return values. The key/values to use for these payloads are exactly the ones you
can find in `fiberplane::models` crate documentation (NOTE: put the docs.rs link
once fiberplane is open-sourced and published).

## `get_supported_query_types`

### Signature

```rust
pub async fn get_supported_query_types(config: ProviderConfig) -> Vec<SupportedQueryType>;
```

### Purpose

Returns the query types supported by this provider.
This function allows Studio to know upfront which formats will be
supported, and which providers (and their query types) are eligible to
be selected for certain use cases.

## `invoke2`

### Signature

```rust
pub async fn invoke2(request: ProviderRequest) -> Result<Blob, Error>;
```

### Purpose

Invokes the provider to perform a data request.

## `create_cells` (Optional)

### Signature

```rust
pub fn create_cells(query_type: String, response: Blob) -> Result<Vec<Cell>, Error>;
```

### Purpose

Creates output cells based on the response.
Studio would typically embed the created cells in the provider cell,
but other actions could be desired.

When any created cells use a `data` field with the value
`cell-data:<mime-type>,self`, Studio will replace the value `self` with
the ID of the cell for which the query was invoked. This allows the
provider to create cells that reference its own data without knowing the
context of the cell in which it was executed.

Note: When the MIME type in the provider response is
`application/vnd.fiberplane.cells` (suffixed with either `+json` or
`+msgpack`), Studio will elide the call to `create_cells()` and simply
parse the data directly to a `Vec<Cell>`.

## `extract_data` (Optional)

### Signature

```rust
pub fn extract_data(response: Blob, mime_type: String, query: Option<String>) -> Result<Blob, Error>;
```

### Purpose

Takes the response data, and returns it in the given MIME type,
optionally passing an additional query string to customize extraction
behavior.

Returns `Err(Error::UnsupportedRequest)` if an unsupported MIME type is
passed.

Note: When the MIME type in the provider response is the same as the
MIME type given as the second argument, and the `query` is omitted, the
return value is expected to be equivalent to the raw response data. This
means Studio should be allowed to elide calls to this function if there
is no query string and the MIME type is an exact match. This elision
should not change the outcome.

## `get_config_schema` (Optional)

### Signature

```rust
pub fn get_config_schema() -> ConfigSchema;
```

### Purpose

Returns the schema for the config consumed by this provider.

Note this schema is only used by Studio to display a configuration form
in case the provider is configured as a direct data source. The provider
itself is responsible for validating the contents of its config.
Assuming the provider uses Serde for parsing the config, validation is
done at that stage.

This function only needs to be implemented by providers that are
statically bundled with Studio.
