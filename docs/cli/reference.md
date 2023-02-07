---
title: Command Reference
category: 63d7e8bf2482560ba8b126e0
slug: command-reference
---
# Command Reference

To list all of the available commands in the CLI type

```bash
fp
```

without any arguments

---

## `fp completions`

Generate fp shell completions for your shell and print to stdout

**Usage**

```bash
fp completions <SHELL>
```

**Example:**

```bash
fp completions zsh # will generate shell completions for zsh
```

---

## `fp data-sources`

Create, view, and update workspace data sources.

**Usage**

```bash
fp data-sources
```

## `fp data-sources create`

Configure a new workspace data source.

Unlike a proxy data source, workspace data sources have their configuration stored in Fiberplane’s API and shared among workspace users. Users’ browsers will query the data source directly.

********Usage********

```rust
fp data-sources create --name NAME --provider-type PROVIDER_TYPE --provider-config PROVIDER_CONFIG
```

- The data source `NAME` must follow [this format](../Quickstart%20cce7b6fa53144989b6835feb900e32dc/Configuration%20help%20FAQ%2018941c30bc32404785f767ab1892c0ce.md) and be unique within the workspace
- The `PROVIDER_TYPE` should be one of `prometheus`, `elasticsearch`, `loki`, etc
- The `PROVIDER_CONFIG` should be a JSON object with a `url` and optional `token` or `username` and `password`

## `fp data-sources defaults`

Set which data sources should be used by default in new notebooks.

Note: only workspace admins can change the default data sources.

**********Usage**********

```rust
fp data-sources defaults set
```

## `fp data-sources list`

List the available data sources for a given workspace. This list includes both workspace data sources (created with `fp data-sources create`) and proxy data sources.

**********Usage**********

```rust
fp data-sources list
```

## `fp data-sources update`

Update the configuration of a workspace data source.

**********Usage**********

```rust
fp data-sources update provider-config --provider-
```

---

## `fp experiments`

Experimental commands to interact with your Fiberplane notebooks 🧪

Run:

```bash
fp experiments
```

or:

```bash
fp x
```

for the full list of available commands.

### `fp experiments crawl`

Crawl and save the provided notebook to a provided directory as Markdown

**Usage**

```bash
fp experiments crawl [OPTIONS] --out-dir <OUT_DIR> <NOTEBOOK>
```

---

## `fp help`

Help provides help for any subcommand in the CLI. Simply type `fp help [subcommmand]` for full details of any given subcommand.

**Usage**

```bash
fp help
```

---

## `fp labels`

### `fp labels list-keys`

Prints a list (or an array) of label keys available in your account.

**Usage**

```bash
fp labels list-keys [OPTIONS]
```

**Options**

- `-o, --output <OUTPUT>` – accepts `list` or `json`, show the output of the command either in list or JSON format.

### `fp labels list-values`

Prints a list (or an array) of label values available for a selected key in your account.

**Usage**

```bash
fp labels list-values <LABEL_KEY> [OPTIONS]
```

**Options**

- `-o, --output <OUTPUT>` – accepts `list` or `json`, show the output of the command either in list or JSON format.

---

## `fp login / logout`

Authenticates with the Fiberplane Studio. `logout` removes the authentication.

**Usage**

```bash
fp login
```

---

## `fp notebooks`

### `fp notebooks create`

Create a notebook from the command line. Returns the URL of the notebook.

**Usage**

```bash
fp notebooks create [OPTIONS]
```

**Options**

- `-t, --title <TITLE>` – Assign a title for the new notebook
- `-l, --label <LABELS>` – Assign labels to the newly created notebook (you can specify multiple labels separated by comma `,`)
- `--from` – assign a start date-time for the notebook (accepts RFC3339 format), e.g.: `2022-05-12T10:56:40Z` . Leave empty for default (relative: 60 minutes ago).
- `--to` – assign an end date-time for the notebok (accepts RFC3339 format), e.g.: `2022-05-12T11:56:40Z`. Leave empty for default (current time).
- `-o, --output <OUTPUT>` - accepts `table` or `json` (default: `table`). Returns the metadata of the notebook created.

### `fp notebooks duplicate`

Duplicates an existing notebook from the command line. Returns the URL of the new notebook. (Once https://github.com/fiberplane/fp/pull/173 is merged)

**Usage**

```bash
fp notebooks duplicate [OPTIONS]
# Aliases
fp notebooks dup [OPTIONS]
fp notebooks clone [OPTIONS]
```

**Options**

- `-n, --notebook-id <ID>` – The ID of the notebook to clone. Will prompt interactively if the option is missing
- `-t, --title <TITLE>` – The title of the new notebook
- `-o, --output <OUTPUT>` - accepts `table` or `json` (default: `table`). Returns the metadata of the notebook created.

**Notes**

- The global argument for the workspace id is taken to be the *target* workspace for the duplicated notebook.

### `fp notebooks delete`

Delete a notebook from the command line. Requires providing the ID of the notebook to be deleted.

**Usage**

```bash
fp notebooks delete <ID>
```

### `fp notebooks get`

Retrieves a notebook from the command line with the provided ID.

**Usage**

```bash
fp notebooks get [OPTIONS] <ID>
```

**Options**

- `-o, --output <OUTPUT>` - accepts `table` or `json` (default: `table`).
    
    > **Note**: Option `table` will print just the metadata of your notebook. If you want to capture the content you will need pass the `json` output.
    > 

### `fp notebooks list`

List all notebooks in your account.

**Usage**

```bash
fp notebooks list [OPTIONS]
```

**Options**

- `-o, --output <OUTPUT>` - accepts `table` or `json` (default: `table`). Prints the list as a table or in JSON format.

### `fp notebooks open`

Opens the notebook in the Studio. Needs an ID of the notebook.

**Usage**

```bash
fp notebooks open <NOTEBOOK_ID>
```

### `fp notebooks search`

Searches for a notebook based on a label

**Usage**

```jsx
fp notebooks search [OPTIONS]
```

**Options**

- `-l, --label <LABEL>` – Search for notebooks with a specific label (format: `key=value`)
- `-o, --output <OUTPUT>` – accepts `table` or `json` (default: `table`). Prints the list as a table or in JSON format.

---

## `fp providers`

Interact with providers.

### `fp providers invoke`

```bash
fp providers invoke [OPTIONS] --provider-path <PROVIDER_PATH> --query-type <QUERY_TYPE> --query-data <QUERY_DATA> --query-mime-type <QUERY_MIME_TYPE> --config <CONFIG>
```

---

## `fp proxies`

Interact with Fiberplane Proxies

### `fp proxies create`

Creates a new proxy.

**Usage**

```bash
fp proxies create [OPTIONS] <NAME>
```

**Options**

- `<NAME>` - will register a name for the proxy (can be left empty)
- `-o, --output <OUTPUT>` - accepts `table` or `json` (default: `table`). Prints the list as a table or in a JSON format.

### `fp proxies data-sources`

List all data sources in Fiberplane.

**Usage**

```bash
fp proxies data-sources [OPTIONS]
```

**Options**

- `-o, --output <OUTPUT>` - accepts `table` or `json` (default: `table`). Prints the list as a table or in a JSON format.

### `fp proxies get`

Retrieves a single proxy. Needs an ID of a Proxy.

```bash
fp proxies get [OPTIONS] <PROXY_ID>
```

**Options**

- `-o, --output <OUTPUT>` - accepts `table` or `json` (default: `table`). Prints the list as a table or in a JSON format.

### `fp proxies list`

Lists all created proxies.

```bash
fp proxies list [OPTIONS]
```

**Options**

- `-o, --output <OUTPUT>` - accepts `table` or `json` (default: `table`). Prints the list as a table or in a JSON format.

### `fp proxies remove`

Removes a proxy. Needs an ID of the Proxy.

```bash
fp proxies remove <PROXY_ID>
```

---

## `fp run`

Run a command and pipe the output to a notebook. This will automatically attempt to parse command output as logs and, if that succeeds, it will insert a log explorer cell in the notebook.

To run a command with pipes, wrap the command in quotes (`fp run “echo hello world | grep hello”`).

Note that shell aliases are not currently supported.

**Usage**

```bash
fp run [OPTIONS] --notebook-id <NOTEBOOK_ID> <COMMAND> [ARGS]...
```

**Options:**

- `-o`,`--output` - Output type to display. Accepts: `command` , `table`, `json` (default: `command`)
- `--notebook-id` - The notebook to append the command to (can be set as an environment variable for the shell `NOTEBOOK_ID`

**Example: Getting Kubernetes Pods**

```bash
fp run --notebook-id <NOTEBOOK_ID> kubectl get pods
```

will send the output of `kubectl get pods` to the provided notebook as a code cell.

**Example: Kubernetes Pod Logs**

```bash
fp run --notebook-id <NOTEBOOK_ID> kubectl logs <POD_ID>
```

will run the command, automatically parse the pod logs, and insert a log cell into the provided notebook.

**Example: Github Action Run Logs**

```bash
fp run --notebook-id <NOTEBOOK_ID> gh run view --log <ACTION_RUN_ID>
```

will fetch the logs of a given Github Action run and insert a log cell into the provided notebook.

---

## `fp templates`

Interact with Fiberplane Templates.

### `fp templates convert`

Create a template from an existing Fiberplane notebook. Needs a notebook ID or URL.

**Usage**

```bash
fp templates convert [OPTIONS] <NOTEBOOK_ID_OR_URL>
```

**Options**

- `--template-name` - Set the name of the template (default: title of the notebook, sluggified)
- `--description` - Set the description of the template (default: empty)
- `--create-trigger` - Create a trigger for this template (default: true)
- `-o, --output <OUTPUT>` - accepts `table` or `json` (default: `table`). Prints the list as a table or in a JSON format.

### `fp templates create`

Create a new template. Needs a path or URL of the template file (`.jsonnet`).

**Usage**

```bash
fp templates create [OPTIONS] --template-name <NAME> <TEMPLATE>
```

**Options**

- `--template-name` - Set the title of the template (default: `template`)
- `--description` - Set the description of the template (default: empty)
- `-o, --output <OUTPUT>` - accepts `table` or `json` (default: `table`). Prints the list as a table or in a JSON format.

### `fp templates expand`

Expand a template into a Fiberplane notebook. 

Needs a name or URL of a template already uploaded to Fiberplane or a path or URL of a template file.

Needs template arguments passed as a comma-separated list of key=value pairs or as a JSON object

**Usage**

```bash
fp templates expand [OPTIONS] <TEMPLATE> [TEMPLATE_ARGUMENTS]
```

### `fp templates get`

Retrieve a single template. Needs the workspace and the name of the template

### `fp templates init`

Initializes a blank template and save it in the current directory as template.jsonnet

### `fp templates list`

List of the templates that have been uploaded to Fiberplane

### `fp templates remove`

Remove a template. Needs the workspace and the name of the template.

### `fp templates update`

Update an existing template. Needs the workspace and the name of the template and a local template `.jsonnet` file

### `fp templates validate`

Locally validate a template file.

Note that you only need to provide template arguments if the template function has required parameters.

**Usage**

```rust
fp templates validate <TEMPLATE PATH> [TEMPLATE_ARGUMENTS]
```

---

## `fp triggers`

Interact with webhook triggers for your Fiberplane Templates.

### `fp triggers create`

---

## `fp tokens create`

Create a token

**Usage**

```bash
fp tokens create [OPTIONS] --name <NAME>
```

**Options**

- `--name <NAME>` Name of the token
- `-o, --output <OUTPUT>` Output of the token [default: table] [possible values: table, json, token]

## `fp tokens list`

Lists all tokens

**Usage**

```bash
fp tokens list [OPTIONS]
```

**Options**

- `--limit <LIMIT>` - Amount of events to display per page
- `-o, --output <OUTPUT>` - Output of the token [default: table] [possible values: table, json]
- `--page <PAGE>` - Page to display
- `--sort-by <SORT_BY>` - Sort the result according to the following field [possible values: title,
created-at, expires-at]
- `--sort-direction <SORT_DIRECTION>` - Sort the result in the following direction [possible values: ascending, descending]

## `fp tokens delete`

Delete a token

**Usage**

```bash
fp tokens delete [OPTIONS] <ID>
```

**Args**

- `<ID>` ID of the token that should be deleted

---

# `fp views`

> Added in `fp v2.2.0`
> 

Interact with Fiberplane view

## `fp views create`

Creates a new view

**Usage**

```bash
fp views create [OPTIONS] [NAME] [DISPLAY_NAME] [DESCRIPTION] [LABELS]…
```

**Args**

- `[NAME]` Name of the view that should be created. This is distinct from `display_name`, which is not constrained
- `[DISPLAY_NAME]` Display name of the view that should be created. This is distinct from `name`, which is constrained
- `[DESCRIPTION]` Description of the view that should be created
- `[LABELS]…` Labels which are associated with this newly created view

## `fp views list`

List views

**Usage**

```bash
fp views list [OPTIONS]
```

## `fp views delete`

Delete a view

**Usage**

```bash
fp views delete --view-name <NAME> [OPTIONS]
```

**Args**

- `<NAME>` Name of the view that should be deleted

## `fp views update`

Update an existing view

**Usage**

```bash
fp views update [OPTIONS] <COMMAND>
```

**Commands**

- `name` Change name of the view
- `display-name` Change display name of the view
- `description` Change description of the view
- `labels` Change Labels

---

## `fp update`

Updates the Fiberplane CLI.

**Usage**

```bash
fp update
```

## `fp version`

Prints the installed version of the Fiberplane CLI

```bash
fp version
```
