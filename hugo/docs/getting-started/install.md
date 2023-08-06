---
title:       Installing the Schematize Hugo Module
linktitle:   Install the Module
description: A documentarian dialect for JSON Schema
summary:     A documentarian dialect for JSON Schema
weight:      1
---

To document and publish schemas with your Hugo site, you need to install the module.

```alert
---
variant: warning
---

In the current release, the Schematize module has only been tested with the
[Platen][w1] module. While the Schematize module's code supports integrating
with Platen, it doesn't _require_ Platen.

[w1]: https://platen.io/modules/platen
```

## Prerequisites

To use the Schematize Hugo module, you need to install:

- Hugo extended for your operating system:
  - [macOS][01]
  - [Linux][02]
  - [Windows][03]
  - [BSD][04]
- [Git][05]
- [Go][06]
- A text editor, like [VS Code][07]

## Steps

1. Update your [site configuration file][08] to add Schematize to import the module:

   ``````tabs { #module-importing }
   ````tab { name="YAML" }
   ```yaml
   module:
     imports:
       - path: github.com/platenio/schematize/hugo
   ```
   ````

   ````tab { name="JSON" }
   ```json
   {
     "module": {
       "imports": [
          { "path": "github.com/platenio/schematize/hugo" }
       ]
     }
   }
   ```
   ````

   ````tab { name="TOML" }
   ```toml
   [[module.imports]]
   path = "github.com/platenio/schematize/hugo"
   ```
   ````
   ``````

1. Run the Hugo command to get the latest version of the Schematize module.

   ```sh
   hugo mod get -u github.com/platenio/schematize/hugo
   ```

1. Verify that the module is installed and part of your site's dependencies.

   ```sh
   hugo mod graph
   ```

## Add the Module Styles

To add the Schematize module's styles, add the following snippet to your HTML head partial:

```go
{{- partialCached "schematize/style/link" "cache" "cache" -}}
```

```alert
---
variant: primary
---

This step is only required for non-Platen sites. For Platen sites, the styles
are automatically loaded.
```

## Next Steps

With the Schematize module installed, you're ready to [add your first schema][09]. You can also
take a look at [configuring the module][10].

<!-- Link references -->
[01]: https://gohugo.io/installation/macos
[02]: https://gohugo.io/installation/linux
[03]: https://gohugo.io/installation/windows
[04]: https://gohugo.io/installation/bsd
[05]:  https://git-scm.com/book/en/v2/Getting-Started-Installing-Git
[06]:  https://go.dev/doc/install
[07]:  https://code.visualstudio.com/download
[08]: https://gohugo.io/getting-started/configuration/#configuration-file
[09]: ./first-schema.md
[10]:../config/site.md
