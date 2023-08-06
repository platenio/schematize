---
title:       Schematize Codeblock
linktitle:   Codeblock
description: A documentarian dialect for JSON Schema
summary:     A documentarian dialect for JSON Schema
---

You can use the `schematize` codeblock markup to document and publish a JSON Schema authored with
the [Schematize dialect][01] to your Hugo site.

When used, it renders the JSON Schema's documentation and publishes the schema to the defined URI
on the site.

## Syntax

``````markdown
```schematize
data_path: <schema dot path>
kind:      <schema kind>
```
``````

## Examples

### 1. Minimal { #example-1 toc_text="1. Minimal" }

In this example, the codeblock is empty.

``````markdown
```schematize
```
``````

With neither the `data_path` nor `uri` options specified, the module looks in its schema map for a
schema that has the `schematize.url` property set to this page's absolute or site-relative URL. If
no schema is mapped for the URL, the module raises an error.

The discovered schema is processed with default options.

### 2. Lookup by Schema Data Path { #example-2 toc_text="2. Lookup by dot path" }

In this example, the codeblock defines only the `data_path` option.

``````markdown
```schematize
data_path: schemas.shop.api
```
``````

The module uses the value of `site.Data.schemas.shop.api` as the schema to document and publish. The
option could also have been specified as `shop/api` for the same effect.

The schema is processed with default options.

### 3. Lookup by Schema URI { #example-3 toc_text="3. Lookup by URI" }

For this example, the codeblock defines the `uri` option.

``````markdown
```schematize
uri: /schemas/shop/v1.2.3/api.json
```
``````

The module looks up the specified URI as the ID of the schema to document and publish. The schema
is processed with default options.

### 4. Document a Dialect { #example-4 toc_text="3. As a Dialect" }

``````markdown
```schematize
data_path: schemas.my_app.dialect
kind:      dialect
```
``````

With the `kind` option set to `dialect`, the schema documentation uses the dialect format instead
of the default. The publishing options are unchanged.

### 5. Document a Vocabulary { #example-5 toc_text="4. As a Vocabulary" }

``````markdown
```schematize
data_path: schemas.my_app.vocab
kind:      vocabulary
```
``````

With the `kind` option set to `vocabulary`, the schema documentation uses the vocabulary format
instead of the default. The publishing options are unchanged.

### 6. Document a Configuration { #example-6 toc_text="5. As a Configuration" }

``````markdown
```schematize
data_path: schemas.my_app.config
kind:      configuration
```
``````

With the `kind` option set to `configuration`, the schema documentation uses the configuration
format instead of the default. The publishing options are unchanged.

## Options

### `data_path`

The `data_path` option defines the lookup path for the schema in your site's data folder. The value
for this option should be the path to the schema file, relative to your data folder. You can
specify the path normally, or using dot-notation.

For example, to document and publish the schema stored in `data/schemas/shop/api.yaml`, you could
specify the data path as `schemas.shop.api`, `schemas\shop\api`, or `schemas/shop/api`.

We recommend using dot-notation, as that's the syntax used in the module's [schema maps][aa].

If the schema lookup resolves to an entry without the [`$id`][ab] keyword defined, the module skips
publishing the schema. It only renders the documentation.

### `uri`

The `id` option defines the lookup for the schema by reference URI. The value for this option must
be a valid URI that resolves to a schema stored in the module's schema map or the subschema of
a mapped schema.

If the schema lookup resolves to an entry without the [`$id`][ab] keyword defined, the module skips
publishing the schema. It only renders the documentation.

### `kind`

The `kind` option defines how the documentation for the schema renders. By default, the module uses
the general schema layout. The module has built-in support for the following kinds:

`configuration`

: For configurations, the module skips the headings for the [`properties`][ac] and
  [`patternProperties`][ad] keywords. Instead, the defined properties and pattern properties are
  added without being in a subsection. Readers of configuration documentation are primarily
  interested in filling out a set of defined properties, not in the implementation details. This
  layout simplifies things for readers and enables a configuration to include more deeply nested
  sets of options than the default schema layout.

  This implementation also skips the sections for the [`default`][ae], [`const`][af], and
  [`enum`][ag] keywords. For configurations, those keywords are almost always scalar values or
  array of scalar values, and they can be represented in the metadata instead of their own section.

`dialect`

: For dialects, the module renders the [`allOf`][ah] keyword as the definition for the dialect's
  vocabularies. At the beginning of the keyword's section, the module generates a table listing
  the dialect's vocabularies and whether integrating tools need to understand those vocabularies.

  After the table, the documentation includes the entries for each of the vocabularies with their
  own heading.

  In the current implementation, only the documentation strings, metadata, the `allOf` keyword's
  section, and any examples are rendered.

`schema`

: For general schemas, the module generates the full documentation for the schema, with a heading
  for each of the defined keywords. No keywords are skipped by default.

`vocabulary`

: For vocabularies, the module renders the metadata, documentation strings, examples, and the
  object keywords. Vocabularies are always defined with their type set to `object` and `boolean`,
  but there's no special keyword handling for boolean type schemas.

A guide on authoring and documenting these schema types is forthcoming. If you don't specify this
option, the default kind is `schema`.

### `schematize`

This option enables you to override the options in the schema's [`schematize`][sa] keyword. After the
module looks up the schema, it merges the properties for this keyword into the schema's definition.
for the keyword. This option's properties are the same as the keyword's properties.

## Notes

[01]: /dialects/v0/compatible
[aa]: ../config/schema_maps.md
[ab]: /vocabularies/core/id
[ac]: /vocabularies/applicator/properties
[ad]: /vocabularies/applicator/patternProperties
[ae]: /vocabularies/meta-data/default
[af]: /vocabularies/validation/const
[ag]: /vocabularies/validation/enum
[ah]: /vocabularies/applicator/allOf
[sa]: /vocabularies/schematize/#properties.schematize.properties
