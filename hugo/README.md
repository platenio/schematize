# hugo-schematize

Tooling for publishing/documenting JSON Schemas from your Hugo site.

## Installing Schematize to Your Hugo Site

This is a hugo module.
You'll need to [make sure your site is ready to add this module to it](https://gohugo.io/hugo-modules/use-modules/).

Once you've done that, you can update your site config to reference it:

```yaml
#config.yaml
module:
  imports:
    - path: 'github.com/platenio/schematize/hugo'
```

```toml
#config.toml
[module]
[[module.imports]]
  path = 'github.com/platenio/schematize/hugo'
```

Once you've saved your updated config file, you can run `hugo mod get -u` to pull this module down for use.
