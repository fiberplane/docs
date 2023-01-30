---
title: Working with templates
category: 63d7e8c259d696004978b93b
---
# Working with templates

<aside>
⚠️ To work with templates you will need to have the Fiberplane CLI installed. See: [CLI](../CLI%20884c99ffa0f5440c9fb2660a32991afb.md).

</aside>

The easiest way to get started with the template is create the structure of the document you want in the app and then in the top right corner click "Share > Download as Template”

![Working%20with%20templates%20d2e5324c62d04eb58eb48f16bfb21b97/Untitled.png](Working%20with%20templates%20d2e5324c62d04eb58eb48f16bfb21b97/Untitled.png)

This will download the existing notebook with the content saved as a Jsonnet file. Alternatively you can use the `fp templates init` CLI command to generate an empty template in your current directory

The template can then be edited as required with a simple text editor (like VSCode), usually if you want to include **parameters** that can be passed in when a template is invoked and notebook is created.

Save the template as a Jsonnet file once you’re happy with it!

## Adding templates to your Fiberplane account

You will need to add your template to your account to start using it in Fiberplane. To add the template run the following command in your terminal (See CLI, to get started with the Fiberplane CLI):

```
fp templates create --title "My hello world template" /path/to/template.jsonnet
```

The `fp templates create` command takes the flag `--title` and the path to your jsonnet file as arguments and adds the template to your Fiberplane account.

You can then verify that the template is added by listing out all of your templates with `fp templates list` or by navigating to your Fiberplane home screen’s *Templates* tab.

## Updating templates

If you have made changes to your Template that you want to upload to Fiberplane you can run the following command:

```
fp templates update <TEMPLATE_ID> --template-path /path/to/template.jsonnet
```

Notice that to update the template you will need to grab the ID of the template you want to change in the first place.

You can look up the template ID either by selecting the template in the app and checking the URL bar (the string following `templateId=`)...

![Working%20with%20templates%20d2e5324c62d04eb58eb48f16bfb21b97/CleanShot_2022-05-03_at_12.26.262x.png](Working%20with%20templates%20d2e5324c62d04eb58eb48f16bfb21b97/CleanShot_2022-05-03_at_12.26.262x.png)

...or in the CLI:

1. List all of your templates with `fp templates list` command.
2. Find your template and copy the ID.

## Removing templates

If you have a template you would like to remove from your gallery, run the following command passing the ID of your template as an argument:

```
fp templates remove <TEMPLATE_ID>
```

These are the basic functions to get you started with Templates in Fiberplane. To see the full reference of available CLI commands see CLI reference.
