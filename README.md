
## This theme expects a few `.Site.Data` parameters

- Hugo provides the content of yaml files (as well as some other data formats) as follows:
  - `docsRoot/data/File.yaml` becomes `.Site.Data.File.DataField.SubField[index].Etc`
- Specifically, this theme uses the following files/parameters
- `data/Solo.yaml` (generated by docs build script in docs dir)
  - DocsVersion: either has the form of "/product/version" or "" empty string
    - "product" could be `gloo`, `squash`, etc
    - "version" will be a sem-ver specification of the form `x-y-z`
  - CodeVersion: either shows the version in the form `x.y.z` or the string `dev`

example:
```yaml
DocsVersion: /gloo/2-3-4
CodeVersion: 2.3.4
```

- `data/ProtoMap.yaml` (generated by solo-kit during code generation)
  - consists of `map[string]Summary`
    - key represents the proto import path (a cannonical UUID)
    - value represents the key info needed about the proto when rendering the docs
      - relativepath: path from the documentation instance's version-scoped root to the served page
      - other fields may be added later

example:
```yaml
apis:
  als.plugins.gloo.solo.io.AccessLog:
    relativepath: api/github.com/solo-io/gloo/projects/gloo/api/v1/plugins/als/als.proto.sk#AccessLog
    package: als.plugins.gloo.solo.io
  als.plugins.gloo.solo.io.AccessLoggingService:
    relativepath: api/github.com/solo-io/gloo/projects/gloo/api/v1/plugins/als/als.proto.sk#AccessLoggingService
    package: als.plugins.gloo.solo.io
```

## This theme fork introduces a few new shortcodes

### protobuf

- `layouts/shortcodes/protobuf.html`
- useful when wanting to link to a protobuf
- parameters:
  - name (required) - import path of the proto
  - display (optional) - text to display on the link, if not specified, defaults to the name

#### Examples

- the following two examples produce the same href: `http://[domain]/[product and version scope]/api/github.com/solo-io/gloo/projects/gloo/api/v1/proxy.proto.sk/#consulservicedestination`

1. link with the default text: "gloo.solo.io.ConsulServiceDestination"

```
{{< protobuf name="gloo.solo.io.ConsulServiceDestination" >}}
```

2. link with custom display text: "consul destination type"

```
{{<
protobuf
name="gloo.solo.io.ConsulServiceDestination"
display="consul destination type"
>}}
```

### versioned_link_path

- `layouts/shortcodes/versioned_link_path.html`
- required in order for links to work (injects version prefix)


#### Examples

- in a markdown link

```
[Gloo Routing]({{< versioned_link_path fromRoot="/gloo_routing" >}})
```

- in an `href`

```
<a href="docker-compose-file"><img src='{{% versioned_link_path fromRoot="/img/docker.png" %}}' width="60"/></a>
```
