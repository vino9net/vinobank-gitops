# Vinobank Demo App

this app requires some external secrets to work.

```shell

# create a scret in GCP Secret Manager as the source of external-secrets
printf '{"newrelic_license_key":"xxxxxNRAL", "aws_access_key_id":"AABBCCDD", "aws_secret_access_key": "XXYYZZ"}' | gcloud secrets create vinobank-dev-secret --project vino9-276317 --data-file=-

```
