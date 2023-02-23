---
title: Sending data from shell to Notebook
category: 63d7e8bf2482560ba8b126e0
slug: fp-run-and-shell-to-cell
---

If you’re an SRE or infrastructure engineer - chances are you spend much of your work time in your shell. Besides querying providers from the Fiberplane notebook, you can easily send your shell comand and output data to a notebook to examine and share with your colleagues.

You can achieve this with the `run` or `shell` commands in the Fiberplane CLI tool.

The `fp run` command will run your specified command in a subshell and send the output to a specified notebook.

The `fp shell` command will start recording and broadcasting your shell session to a specified notebook. It is useful for peer debugging sessions or simply when you want to have an easily accessible command history log for debugging documentation.

### Initial setup

Make sure you have the Fiberplane CLI installed and you’re logged in. See: [CLI](doc:cli) for installation instructions.

## Run `fp run`

You can now run your shell command and send the output to the notebook.

```bash
fp run <YOUR_SHELL_COMMAND> -- [ARGS_OF_YOUR_SHELL_COMMAND]
```

Note the `--` supplied before a shell command argument. This is so you can
separate flags and arguments intended for the `fp` command and for the shell command you intend to run.

Fiberplane CLI creates a subshell and runs your command, capturing the
timestamp, the working directory, the command, and the output, and piping this data to the notebook.

You can now share the status of your pods and services directly into the notebook by running something like:

```bash
❯ fp run podman logs <pod_id>
```

And here’s how that will look like in the notebook:

![Untitled](/docs/cli/fp-run/Untitled.png)

## Using a specific notebook section

By default, Fiberplane will add your command output to a new code cell in the notebook. However, you can also specify a specific notebook section to send your output to.

```bash
> fp run <YOUR_SHELL_COMMAND> --send-to <NOTEBOOK_ID> --section <SECTION_NAME>
```

## Customizing the notebook cell metadata

You can customize the metadata of the notebook cell that Fiberplane creates for your command output. This can be useful if you want to add tags, hide the cell input, or add custom styling.

To customize the metadata, you can create a JSON file with the metadata and pass it to the fp run command using the --metadata-file option:

```
> fp run <YOUR_SHELL_COMMAND> --send-to <NOTEBOOK_ID> --metadata-file <METADATA_FILE_PATH>
```

Here's an example metadata file:

```
{
  "tags": [
    "my-tag"
  ],
  "hide_input": true,
  "trusted": true,
  "deletable": false,
  "editable": false,
  "celltoolbar": "Tags",
  "metadata": {
    "my-custom-key": "my-custom-value"
  }
}
```

## Sending commands from a file

If you have a long or complex shell command that you want to send to a notebook, you can put it in a file and use the fp run command to send the contents of the file to the notebook.

```
fp run "$(cat <FILE_PATH>)" --send-to <NOTEBOOK_ID>
```
## Rich output

Currently all of the commands and their output is sent to a simple code cell.

Large text outputs such as logs are easier to parse and read in a dedicated UI as opposed to in a stream of text. Fiberplane will recognize some of these commands and will apply the appropriate formatting for logs (currently supported for Kubernetes and Github Actions logs).

## Run `fp shell`

You can now start recording and broadcasting your shell session to a specified notebook.

```bash
fp shell [OPTIONS] <NOTEBOOK_ID>
```

Fiberplane CLI will establish a secure channel and will start broadcasting your shell commands and their output to the notebook live. Each new command will extend the cell and append the result.

Hit CTRL-D to exit the recorded session and stop the recording. The notebook will save the start and end times of the recorded shell session.

> 💡 Note
>
> `fp shell` will only work with non-interactive shell programs (e.g.:
> it will not display vim in there)
