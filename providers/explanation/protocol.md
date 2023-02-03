---
title: Provider Protocol Explanation
category: 63d7e8bdbf7b4b0e0745e823
slug: provider-protocol-explanation
---

# Provider Protocol Flow

This page explains how the interactions between Fiberplane Studio and a provider
happen to interactively add cells to a notebook. It does _not_ touch at all how
to write a provider.

You should know what a notebook and cells are in Fiberplane context.

In the following page, a few words are important to define:
- **Data Source**: a data source is a single configured instance of a provider.
  For example, you can configure 2 Prometheus data sources with the Prometheus
  Provider, each talking to a different instance.
- **Third Party Resource**: the external resource the data source is configured
  to talk to. For example, for the Prometheus provider, the third-party resource
  is the actual instance of Prometheus.
- **SlashCommand**: a command that gets activated from typing `/` in a notebook.
- **Blob**, **ProviderRequest**: data types from the [provider protocol](../reference/protocol.md)

## Summary sequence diagram

In the sequence diagram below, all "critical" sections are named after one of the functions of the
[provider protocol](../reference/protocol.md).

``` mermaid
sequenceDiagram
    actor user as User
    participant studio as Fiberplane Studio
    participant ds as Data Source<br />(Provider Instance)
    participant tpr as Third-party Resource
    participant api as Fiberplane API
    user->>studio: Connect and create notebook
    ds->studio: Registration of providers (through Proxy or Direct data source)
    critical get_supported_query_types
        studio->ds: list of SupportedQueryTypes and<br />generation of slashCommands
    end
    studio->>user: Show list of supported queries<br />as slashCommands
    user->>+studio: Choose a slashCommand to<br />activate integration
    studio->>studio: Find the correct data source<br />for a give query type
    critical get_supported_query_types
        studio->>+ds: Request with "provider_type.query_type"
    end
    ds->>-studio: List of QueryFields to fill
    studio->>-user: Show query fields
    user->>+studio: Fill query fields<br />(user action)
    critical invoke2
        studio->>+ds: ProviderRequest<br />(query_type + url-form-encoded fields)
        ds->tpr: Request
        ds->>ds: Serialize inner data model to Blob
        ds->>-studio: Blob response
    end
    par Save the state of the notebook
        studio->>api: Storage of Blob as notebook metadata
    and Create new cells for studio
        alt application/vnd.fiberplane.cells{+json,+msgpack}
            studio->>studio: decode and create cells
        else create_cells
            studio->>+ds: Create Cells<br />(original query + blob)
            ds->>ds: Deserialize inner data model from Blob
            ds->>-studio: Cell commands
        end
        opt Graph or Log cell<br/>with complex MIME type
            critical extract_data
                studio->>+ds: Extract data from the cell "data-links"
                ds->>ds: Extract relevant MIME type from inner data model
                ds->>-studio: Cell-formatted data
            end
        end
    end
    studio->>-user: Display cells
```

## Adding data sources

Before Studio can allow users to run actions connected to a third party
resource, it needs to know about the available data sources. There are 2 ways a provider
can get registered to create data sources.

### Built-in Fiberplane providers

When Fiberplane builds and adds a provider integration first hand, we guarantee
that Studio will know about it and be able to create commands and data sources
from the provider.

In order to add a data source in Studio, you can follow the [Adding a data source](./TODO) tutorial.

### Custom providers (through Fiberplane Daemon)

Fiberplane allows you to use your custom providers in your workspace, using
Fiberplane Daemon to proxy the communication between your own WebAssembly
provider and Fiberplane Studio. When starting, Fiberplane Daemon will
automatically tell Fiberplane Studio about the data sources that are configured
with `fpd` configuration file.

## Lifecycle of data source interaction

### Listing and choosing associated slash command

Once a data source is added to a notebook, Fiberplane Studio will use
`get_supported_query_types` call to list all the commands/requests that the
backing provider supports. This query returns a list of queries with their
names, and the input schema of the query (the list of field names and types to
fill to make a valid query).

Once a slash command is chosen by a user, Studio expands the input schema into
input fields to call the provider.

### Invoking the provider and Showing results

Once the input of a command is filled, any user can "Run" the cell in order to
make the data source do its magic. When a user hits "Run", Studio actually
serializes the fields into a `ProviderRequest` and uses that to call the
`invoke2`[^invoke2name] method of the data source to obtain results.

[^invoke2name]: `invoke2` has an extra suffix because it's the second version
    of the function

#### Data sources return Blobs

Data sources are allowed to return a `Blob` of any MIME type to an invocation.
Most of the times the MIME type will be unknown to Studio, so extra exchanges
between Studio and the data source need to happen; but a few MIME type is
special, because Studio already knows exactly how to render those blobs in a
notebook: `application/vnd.fiberplane.cells` is an array of formatted
`fiberplane::models::Cell` that will immediately get rendered by Studio.

#### Studio asks how to create cells

More often than not, data sources will return specific Blobs that Studio cannot
natively deal with, e.g.
`application/vnd.fiberplane.providers.cloudwatch.query-results`. In that case
Studio needs to know how to display the results in the notebook.

Studio will call `create_cells` on the data source, sending back the `Blob` it
received. The data source can then use the query_type information as well as the
Blob in order to return a list of `fiberplane::models::Cell` that Studio can
then natively use to show the results in a pretty fashion in the notebook.

#### The case of data-heavy cells (data extraction)

Cell types in `fiberplane::models::Cell` usually own their underlying data. For
example, a `TextCell` structure will embed the actual text that should be
displayed within it.

A few cell types do _not_ own their underlying data, mostly to avoid sending
back and forth huge payloads to describe the cells. Currently the main cells
that do not own their data are the [Log Cell](./TODO) and the [Graph
Cell](./TODO). Instead of having an array of events or an array of timeseries
within, they use a property called `data-links` to describe where Studio should
find the relevant data for the Cell.

When you start creating a Provider, you will want to use `self` for the location
of the underlying data, which tells Studio "The data you want for the log/graph
cell is exactly the response from the current slash command the user did".

But Studio also expects the data to follow specific MIME types to be used in the
cells: `application/vnd.fiberplane.events` for Log cells, and
`application/vnd.fiberplane.timeseries` for Graph cells. Most of the time, the
data returned from the invocation will not match that specific format (it will
be a provider-specific blob like
`application/vnd.fiberplane.providers.cloudwatch.query-results`). So how can
Studio obtain the relevant data for a Log or Graph cell out of custom blobs?
This is what the `extract_data` call is supposed to solve.

When Studio wants to display a Cell that uses a `data-link` to find its data, Studio will:
- read the MIME type to extract from the data-link (usually
  `application/...events` or `...timeseries`),
- read the location of the cell to extract data from (usually `self`), and
- call `extract_data` with the target MIME type on the data-source that owns the
  cell (so usually the same cell than the one that gave the Log/Graph cell in
  the earlier `create_cells` call)

### Results in notebook

Once Studio did all the calls to obtain interactive cells in the notebook, the
underlying data gets synchronized with our backend, and the state of the cells
and the notebook gets shared between all users of the notebook so everyone can
look at the data and interact with it in front of everyone!
