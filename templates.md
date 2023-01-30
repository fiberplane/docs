---
title: Templates
category: 63d7e8c259d696004978b93b
slug: templates
---
# Templates

Programmatically create Fiberplane Notebooks for repeatable workflows.

---

# Overview

Fiberplane Templates are programmable workflows that allow you to automate notebook creation for your incident response, infrastructure debugging, and maintenance.

## **Overview of a template**

Templates are defined by a Jsonnet file that is added to your Fiberplane account through the CLI.

Templates export a Jsonnet function that accepts some parameters and creates a Notebook using the helper functions provided by the Fiberplane Jsonnet library (fiberplane.libsonnet). See an example template below:

```bash
local fp = import 'fiberplane.libsonnet';
local c = fp.cell;

function(
    incidentName='Service Outage'
)
  fp.notebook
    .new('Incident Response for: ' + incidentName)
    .setTimeRangeRelative(minutes=60)
    .addCells([
      c.text('Hello World!')
    ])
```

Let’s break it down line by line:

| local fp = import 'fiberplane.libsonnet';
local c = fp.cell; | Imports a helper Jsonnet library for working with Fiberplane notebooks. |
| --- | --- |
| function (
    incidentName='API Outage'
) | Calls a Jsonnet function passing a string ‘API Outage’ as a parameter incidentName. |
| fp.notebook | Initiates a Fiberplane notebook. |
| .new('Incident Response for: ' + incidentName) | Calls a method that adds a title for the notebook passing a harcoded string and the parameter (in this case, string as well) concatenated. |
| .setTimeRangeRelative(minutes=60) | Sets the time range for the notebook to be relative to last hour |
| .addCells([
     c.text('Hello World!')
]) | Calls a method to add cells to the notebook and adds a simple text cell with a string “Hello World!” |

We'll cover each of these in greater detail in the guide below. By the way: you can find more practical, use-case specific template examples in our [Quickstart repo in the "templates/"](http://github.com/fiberplane/quickstart) subdirectory

[Working with templates](Templates%201a94513cc97445459eb6c5789210dc13/Working%20with%20templates%20d2e5324c62d04eb58eb48f16bfb21b97.md)

[Working with parameters](Templates%201a94513cc97445459eb6c5789210dc13/Working%20with%20parameters%20902684ce8c6443d19bad01a57e844e8e.md)

[Working with triggers](Templates%201a94513cc97445459eb6c5789210dc13/Working%20with%20triggers%20173525d4dc9b4ff8af4dca5c0e30f691.md)

[PagerDuty Integration](Templates%201a94513cc97445459eb6c5789210dc13/PagerDuty%20Integration%20684aba36f974468093868cba513b6b38.md)

---
