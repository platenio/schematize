---
title:       Adding Your First Schema
linktitle:   Add a Schema
description: A documentarian dialect for JSON Schema
summary:     A documentarian dialect for JSON Schema
weight:      2
---

With the Schematize dialect and Hugo module, you can document and publish your schemas from a single source.

In this tutorial, you'll author a short JSON schema in the Schematize dialect, then use Hugo to
publish the schema and its documentation together.

## Prerequisites

This tutorial assumes that you've created a Hugo site and installed the Schematize Hugo module. If
you haven't done so yet, follow the steps in [Installing the Schematize Hugo Module][01].

```alert { variant="primary" }
This tutorial uses VS Code as the editor for the files. You can use any text
editor for the tutorial, but VS Code's YAML language server enables you to
author your schema with auto-complete and validation.
```

## Add the Schema

To publish a schema, you need to define it as a data file. In your site's data directory, create
the `schemas/order.yaml` file and open it in your editor.

```sh
mkdir data/schemas
touch data/schemas/author.yaml
code  data/schemas/author.yaml
```

Add the following to the YAML file:

```yaml
# yaml-language-server: $schema=https://schematize.platen.io/v0/dialects/enhanced.json
$schema:     https://json-schema.org/draft/2020-12/schema
$id:         /schemas/author.json
title:       Published Author Schema
description: Defines an author of published works.

type:        object
required:
  - name
  - works
properties:
  name:
    title:       Author Name
    description: Defines the author's given, family, full, and additional names.
    type:        object
    required:
      - full
    properties:
      full:
        title:       Full Name
        description: Defines the author's full name.
        type:        string
      given:
        title:       Given Name
        description: Defines the author's given name.
        type:        string
      family:
        title:       Family Name
        description: Defines the author's family name.
        type:        string
      additional:
        title:       Additional Names
        description: >-
            Defines a list of additional names for the author, like nicknames
            or pseudonyms.
        type:        array
        uniqueItems: true
        items:
          type: string
  works:
    title:       Published Works
    description: Defines the list of works the author has published.
    type:        array
    uniqueItems: true
    minItems:    1
    items:
      title:       Published Work Item
      description: An author's published work.
      type:        object
      properties:
        title:
          title:       Published Title
          description: Defines the work's title.
          type:        string
        date:
          title:       Published Date
          description: Defines the work's publishing date.
          type:        string
          format:      date
        type:
          title:       Work Item Type
          description: Defines the work's type.
          type:        string
        publisher:
          title:       Publisher
          description: Defines the name of the work's publisher.
          type:        string
```

In your site's content folder, create a new markdown file called `authors.md`. You can put the file
anywhere, but this example places it in the `schemas` folder.

```sh
touch ./schemas/author.md
code ./schemas/author.md
```

Add the minimal front matter to the new content file and the `schematize` codeblock.

``````markdown
---
title: Author
---

```schematize
data_path: schemas.author
```
``````

Serve your site locally.

```sh
hugo server
```

Navigate to the newly created page for the schema, like `http://localhost:1313/schemas/author`. You
should see the automatically generated documentation for the schema on that page.

Navigate to the URI for the schema's ID, like `http://localhost:1313/schemas/author.json`. You
should be able to review or download the schema from that URI.

## Document the Schema

Review the rendered documentation for the schema. It includes the `title` and `description` for
each entry in the schema, plus the relevant metadata. However, the short strings used for the
entry descriptions don't provide much more than high-level context.

Start by adding the `schematize` keyword with the `description` property to the schema's top-level,
after the `title` and `description` keywords.

```yaml
title:       Published Author Schema
description: Defines an author of published works.
schematize:
  description: |
    This schema defines an author of published works. An author entry always
    defines the author's name and the list of their works.

    Author entries defined by this schema are used in the database that powers
    the site's search functinality. After an author's information is added,
    they show up in search results.
```

Notice that the description section of the document updates to the new text. You can define this
keyword for any entry in the schema. You can also use any of the other [Schematize keywords][02] to
extend the documentation or change how it renders.

## Review and Next Steps

In this short tutorial, you defined a new schema and added it to your site. You saw how the schema
and its documentation were automatically published and extended the documentation.

Try adding one of your own schemas and using the [Schematize keywords][02] to document it.

[01]: ./install.md
[02]: /vocabularies/schematize
