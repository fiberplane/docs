---
title: CLI
category: 63d7e8bf2482560ba8b126e0
slug: cli
---
# CLI

Fiberplane CLI is a command-line tool that helps you install Proxies, set up Providers data sources, create and manage Notebooks and Templates all from your terminal.

---

[Command Reference](cli/reference) 

## Getting started with the CLI

### Installing

Download and install the Fiberplane CLI

```bash
curl --proto '=https' --tlsv1.2 -sSf https://fp.dev/install.sh | sh
```

Or download the Fiberplane CLI manually (make sure to make it executable):

- [Linux X86](https://fp.dev/fp/latest/x86_64-unknown-linux-gnu/fp)
- [MacOS Arm64](https://fp.dev/fp/latest/aarch64-apple-darwin/fp)
- [MacOS X86](https://fp.dev/fp/latest/x86_64-apple-darwin/fp)

Authenticate your CLI with Fiberplane

```bash
fp login
```

Verify that Fiberplane CLI is working correctly by typing fp without any arguments. You should see a list of available commands.

![og-image_Best practices for observability (13).png](./cli/og-image_Best_practices_for_observability_(13).png)

### Updating

To update simply type:

```bash
fp update
```

---

## Guides

[Create Proxies with the CLI](Quickstart%20cce7b6fa53144989b6835feb900e32dc.md)

[Create Templates with the CLI](Templates%201a94513cc97445459eb6c5789210dc13/Working%20with%20templates%20d2e5324c62d04eb58eb48f16bfb21b97.md)

[Create Webhook triggers with the CLI](Templates%201a94513cc97445459eb6c5789210dc13/Working%20with%20triggers%20173525d4dc9b4ff8af4dca5c0e30f691.md)

[`fp run and` Shell to Cell](CLI%20884c99ffa0f5440c9fb2660a32991afb/fp%20run%20and%20Shell%20to%20Cell%20704e1a1e5b4c4f1691c1683f2177f933.md)
