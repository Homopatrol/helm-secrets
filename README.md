# Helm-secrets
An example of how to handle secrets outside of a child chart.

## Why would I need to do this?

Child charts such as Postgresql do not have the ability to reuse it's Chart generated secret when performing a Helm Upgrade
ie. The first password generated is the correct value and after re-installing, the secret would get populated with a new (wrong) password.


<!-- Additionally, by adding options to re-create the secrets, it's easier to handle the lifecycle of the secrets: ie. the same secret when performing an upgrade so it will still hold the correct value. -->


   <!-- the "existingSecret" parameter assumes that we are creating the secret outside Helm. -->

By handling secrets outside of the chart this allows us to create a secret outside of the Postgresql Chart and add additional functionality so the secret will persist when perfroming a Helm Upgrade.
