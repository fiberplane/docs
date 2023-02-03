---
title: Creating a provider
category: 63d7e8bdbf7b4b0e0745e823
slug: creating-a-provider-advanced
---

# Create a provider (advanced topics)

This builds on the [Creating a provider tutorial](doc:creating-a-provider), and touches additional topics

## Implementing Status query

Advanced page

## Implementing Suggestions query

Advanced page

## Adding a query that returns more than just cells

### For an _advanced_ tutorial later
On top of choosing the name, we will also choose the inner data model of
"Closest Dispenser" response. Here the goal is to choose a structure that is
easy to manipulate in Rust; we will be using this structure as the basis to
create cells in a notebook.

```rust
pub const CATNIP_CLOSEST_MIME_TYPE: &str = "application/vnd.fiberplane.providers.catnip.closest";

// We are using the `ProviderData` helper macro to do the heavy work
#[derive(Deserialize, ProviderData, Serialize, Debug, Clone)]
#[pdk(mime_type = CATNIP_CLOSEST_MIME_TYPE)]
struct ClosestDispenserData {
    target: GeoLocation,
    distance: Option<f64>,
    user: Option<User>
}
```

- Implementing the handler for `create_cells`
- Implementing the handler for `extract_data`

