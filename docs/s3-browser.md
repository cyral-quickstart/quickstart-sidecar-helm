#### Enable the S3 File Browser

To configure the sidecar to work on the S3 File Browser, set the following extra parameters under the service key of your values file:

```
service:
  dnsName: "<CNAME>" # ex: "sidecar.custom-domain.com"
  loadBalancer:
    tlsPorts: [443]
    certificateId: "arn:aws:acm:<REGION>:<AWS_ACCOUNT>:certificate/<CERTIFICATE_ID>"
```

The CNAME provided in `service.dnsName` must be created
after the deployment pointing to the sidecar load balancer.
See [Add a CNAME or A record for the sidecar](https://cyral.com/docs/sidecars/manage/alias).

All the Helm parameters used above are documented in the 
[values file configuration reference](./values-file.md). 
For more details about the S3 File Browser configuration, check the 
[Enable the S3 File Browser](https://cyral.com/docs/how-to/enable-s3-browser) 
documentation.
