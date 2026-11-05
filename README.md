# files-context-transport-logistics

A Meltano [file bundle](https://docs.meltano.com/concepts/plugins/#file-bundles) of **industry
context for transport and logistics**. Meltano Cloud adds it to a workspace whose organisation's industry
is Transport & Logistics.

The bundle holds datasets with no query. Each one is a written briefing: the segments and source
systems of the industry, its core entities, the metrics it runs on, how to model its data in dbt,
and how to handle its data. They are written for an AI assistant (or an analyst) working in the
workspace, for example one building dbt models on raw shipment and fleet data.

```
analyze/datasets/transport-logistics/
  overview.yml
  data-model.yml
  metrics.yml
  dbt-modelling.yml
  data-handling.yml
```

## Organisation knowledge: `#organisation-context`

Every dataset here is tagged `#organisation-context`, the tag of an organisation's knowledge in Meltano
Cloud. The `get_organisation_knowledge` MCP tool returns the full text of every dataset with it, for an
AI assistant to read before it models, transforms or queries the workspace's data. In the repository,
`grep -rl '#organisation-context' analyze/datasets` finds the same set. Tag a dataset of your own the
same way to add it.

## Install

Meltano Cloud installs it when Transport & Logistics is chosen as the workspace's industry, in the
setup wizard or with the `set_organisation_context` MCP tool.

In a Meltano project of your own, save this definition to a file outside the project, for example
`/tmp/files-context-transport-logistics.yml`:

```yaml
name: files-context-transport-logistics
namespace: files_context_transport_logistics
variant: matatika
pip_url: git+https://github.com/meltano/files-context-transport-logistics.git@v0.1.1
update:
  '*.yml': true
```

then add the bundle from the project directory. `meltano add` writes the datasets; a plain
`meltano install` does not:

```sh
meltano add --plugin-type files files-context-transport-logistics --from-ref /tmp/files-context-transport-logistics.yml
```

`update: '*.yml': true` lets `meltano upgrade files` refresh the datasets from a newer version.

## Release

Tag a version (`git tag -a v0.1.1 -m v0.1.1 && git push --tags`) and bump the `pip_url` ref in the
Meltano Cloud registry definition (`plugins/files/files-context-transport-logistics--matatika.yml`).

These datasets are general industry context, not legal or regulatory advice.
