# Helm-secrets
A Postgresql example of how to handle secrets outside of a subchart in a Parent chart

## Why would I need to do this?
When you Create a chart with dependencies to other charts which are completely external these are refered to as "subcharts"

See [helm - subcharts and globals](https://helm.sh/docs/chart_template_guide/subcharts_and_globals/)

>"A subchart is considered "stand-alone", which means a subchart can never explicitly depend on its parent chart.     
> For that reason, a subchart cannot access the values of its parent.   
> A parent chart can override values for subcharts."

In my example, Postgresql is my subchart. Therfore it does not have the ability to reuse it's Chart generated secret when performing a Helm Upgrade
ie. The first password generated is the correct value and after re-installing, the secret would get populated with a new (wrong) password.

<!-- Additionally, by adding options to re-create the secrets, it's easier to handle the lifecycle of the secrets: ie. the same secret when performing an upgrade so it will still hold the correct value. -->


   <!-- the "existingSecret" parameter assumes that we are creating the secret outside Helm. -->

By handling secrets outside of the chart this allows us to create a secret outside of the Postgresql Chart and add additional functionality so the secret will persist when perfroming a Helm Upgrade.

### Using Dependencies
To create a dependency to another chart you must use `Chart.yaml`

An example Chart.yaml file looks like:
```
apiVersion: v2 # The chart API version 
name: myChart # The name of the chart
version: 1.0.4 # A SemVer 2 version (required)
appVersion: 1 # The version of the app that this contains
description: This is my example chart.yaml file # A single-sentence description of this project
home: # The URL of this projects home page
icon: # A URL to an SVG or PNG image to be used as an icon 
keywords: # A list of keywords about this project
- security
- postgresql
sources: # A list of URLs to source code for this project (optional)
dependencies: (optional) # this is where you include your subcharts
- condition: postgresql.enabled # A yaml path that resolves to a boolean, used for enabling/disabling charts 
  name: postgresql # The name of the chart
  repository: https://charts.bitnami.com/bitnami # The repository URL
  version: ~10.3 # The version of the chart 
  tags: # Tags can be used to group charts for enabling/disabling together
    import-values: # holds the mapping of source values to parent key to be imported. Each item can be a string or pair of child/parent sublist items.
    alias: # Alias to be used for the chart. Useful when you have to add the same chart multiple times
maintainers: # (optional)
  - name: Homopatrol (required for each maintainer)
    email: PandoraH@protonmail.com (optional for each maintainer)
    url: https://github.com/Homopatrol (optional for each maintainer)
deprecated: # Whether this chart is deprecated (optional, boolean)
```
Now you can use Postgresqls chart in your parent chart.

You can read more about configuring your chart.yaml in the [helm documentation](https://helm.sh/docs/topics/charts/)

### Configuring Subcharts 

values.yaml
```
Postgresql:
  enabled: true
```
