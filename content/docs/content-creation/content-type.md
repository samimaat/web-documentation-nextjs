---
title: Content types
slug: content-creation/content-types
description: null
date: 2022-03-14T08:43:17.483Z
lastmod: 2022-09-22T07:21:54.904Z
weight: 200.1
---

# Content types

## Changing the default content type

If you want to change the default content type, open your `frontmatter.json` and write an entry for the `frontMatter.taxonomy.contentTypes` setting. Visual Studio Code will automatically autocomplete it with the default content type fields.

If in some case it wouldn't do this, here is the default content type structure:

```json
"frontMatter.taxonomy.contentTypes": [
  {
    "name": "default",
    "previewPath": null,
    "pageBundle": false,
    "fields": [
      {
        "title": "Title",
        "name": "title",
        "type": "string"
      },
      {
        "title": "Description",
        "name": "description",
        "type": "string"
      },
      {
        "title": "Publishing date",
        "name": "date",
        "type": "datetime",
        "default": "{{now}}",
        "isPublishDate": true
      },
      {
        "title": "Article preview",
        "name": "preview",
        "type": "image"
      },
      {
        "title": "Is in draft",
        "name": "draft",
        "type": "draft"
      },
      {
        "title": "Tags",
        "name": "tags",
        "type": "tags"
      },
      {
        "title": "Categories",
        "name": "categories",
        "type": "categories"
      }
    ]
  }
]
```

Adapt the fields to your needs. For our documentation it looks as follows:

```json
"frontMatter.taxonomy.contentTypes": [
  {
    "name": "default",
    "previewPath": null,
    "pageBundle": false,
    "fields": [
      {
        "title": "Title",
        "name": "title",
        "type": "string"
      },
      {
        "title": "Description",
        "name": "description",
        "type": "string"
      },
      {
        "title": "Publishing date",
        "name": "date",
        "type": "datetime",
        "default": "{{now}}",
        "isPublishDate": true
      },
      {
        "title": "Last modified date",
        "name": "lastmod",
        "type": "datetime",
        "isModifiedDate": true
      },
      {
        "title": "Navigation weight",
        "name": "weight",
        "type": "number"
      }
    ]
  }
]
```

The metadata section on the editor panel will render the following fields:

![Adapted default content type fields](/assets/adapted-default-ct.png)

### Content type properties

For the content type you can configure the following properties:

| Property | Type | Description | Default value |
| --- | --- | --- | --- |
| `name` | `string` | Name of the content type | `""`  |
| `fields` | `array` | Check the [supported field types](/docs/content-creation/fields#supported-field-types) | `[]` |
| `fileType` | Enum: `md, mdx, markdown, <your choice>` | File type of for the content type you define. The type will be used to create the file when creating content. | `md` |
| `pageBundle` | `boolean` | If set to `true`, the content will be created as a page bundle (folder). | `false` |
| `previewPath` | `string` | Defines a custom preview path for the content type. When the preview path is not set, the value from the [frontMatter.preview.pathName](https://frontmatter.codes/docs/settings#frontmatter.preview.pathname) setting will be used. | `null` |
| `template` | `string` | Specify a path to a template file that will be used when creating new content with the content type. | `null` |
| `postScript` | `string` | An optional post script that can be used after new content creation. In order to use this, you will have to set the value to the ID of your [content script](/docs/custom-actions/#content-script). | `null` |

## Define your own type

In most cases, you'll work with multiple types of content where each type will have its defined set of fields. If this is the case for your website, you'll need to add another content type to the `frontMatter.taxonomy.contentTypes` setting.

Instead of overriding the default content type, you can also define a new content type. It requires you to specify the `name` and `fields`.

> **Important**: The `name` property value needs to be equal to the `type` value you set in your Markdown front matter. You best define it via a template so that it's always defined. A default template will be available when initializing Front Matter in your project in the `.frontmatter/templates` folder.

Sample:

```json
"frontMatter.taxonomy.contentTypes": [
  {
    "name": "default",
    "fields": [
      ...
    ]
  },
  {
    "name": "documentation",
    "fields": [
      ...
    ]
  }
]
```

In the sample, `documentation` is used as the content type name. This means that in your article, you'll need to add the `type: documentation` to your front matter to let the editor panel understand which fields to show.

```markdown
---
title: Content Types
slug: content-types
description: null
date: '2021-09-17T07:36:26.654Z'
lastmod: '2019-08-22T15:20:28.000Z'
weight: 4
type: documentation
---
```

> **Fields**: Check out the [fields](/docs/content-creation/fields) section to learn which fields are supported.


## Using a template with the content type

When creating content that requires a pre-defined structure, you can use the `template` property. The `template` property allows you to link a template file. On content creation, the contents of the template file will be used to create the new content.

You can use the `template` property on any of your content types, even on the default content type from Front Matter.

```json
{
  "frontMatter.taxonomy.contentTypes": [
    {
      "name": "default",
      "pageBundle": false,
      "template": "[[workspace]]/.frontmatter/templates/article.md",
      "fields": [...]
    }
}
```

## Run a script after your content is created

To run a custom script after your content is created, you can use the `postScript` property. The `postScript` property allows you to link a script file by referencing its `id`. The script will be executed after the content is created which gives you access to the whole front matter object.

```json
{
  "frontMatter.taxonomy.contentTypes": [
    {
      "name": "default",
      "pageBundle": false,
      "template": "[[workspace]]/.frontmatter/templates/article.md",
      "fields": [...],
      "postScript": "generate-social-image"
    }
  ]
}
```

In your custom script setting, you need to make sure that this script is available:

```json
{
  "frontMatter.custom.scripts": [
    {
      "id": "generate-social-image",
      "title": "Generate social image",
      "script": "./scripts/social-img.js",
    }
  ]
}
```

> **Info**: More information about custom scripts can be found in the [custom actions](/docs/custom-actions) section.