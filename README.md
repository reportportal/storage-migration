# Report Portal storage migration

Report Portal storage migration:

1. From multiple buckets to single bucket for S3 and MinIO.
2. From single bucket MinIO to single bucket S3.

# Prerequisites
* Kubernetes 1.19+
* Helm 3.2.0+

# Installing the Chart

Add the Storage Migration Helm charts repository

```
helm repo add rp-storage-migration https://hlebkanonik.github.io/rp-storage-migration/
```

To install the chart with the release name `my-release`. For Report Portal storage migration from multiple buckets to single bucket for S3 and MinIO

```
helm install my-release rp-storage-migration/rpsm-multi-single
```

For Report Portal storage migration from single bucket MinIO to single bucket S3

```
helm install my-release rp-storage-migration/rpsm-s3-minio
```

# Storage Migration guide

1. Uninstall Report Portal Helm chart `helm uninstall my-release`
2. Add Storage Migration repository
```
helm repo add https://hlebkanonik.github.io/rp-storage-migration/
```
3. To migrate data from multiple buckets to single bucket deploy `rp-storage-migration/rpsm-multi-single` Helm chart. Use [values.yaml](https://github.com/hlebkanonik/rp-storage-migration/blob/main/rpsm-multi-single/values.yaml) to pass your values. [More info](https://github.com/hlebkanonik/rp-storage-migration/blob/main/rpsm-multi-single/README.md)
> ❗️ Data migration depending on the size of your storage (~14Gb per hour).
```
helm install rpsm-multi-single -f values.yaml rp-storage-migration/rpsm-multi-single
```
> ❗️ You need to wait until the end of the migration
3. When the data has been successfully migrated, Job.butch will get the status `Completed`. Uninstall `rp-storage-migration/rpsm-multi-single`
```
helm uninstall rpsm-multi-single
```
4. Create a bucket in Amazon S3. [Create your first S3 bucket](https://docs.aws.amazon.com/AmazonS3/latest/userguide/creating-bucket.html)
5. To migrate data from single bucket MinIO to single bucket S3 deploy `rp-storage-migration/rpsm-s3-minio` Helm chart. Use [values.yaml](https://github.com/hlebkanonik/rpsm-s3-minio/blob/main/rpsm-multi-single/values.yaml) to pass your values. [More info](https://github.com/hlebkanonik/rpsm-s3-minio/blob/main/rpsm-multi-single/README.md).
```
helm install rpsm-s3-minio -f values.yaml rp-storage-migration/rpsm-s3-minio
```
> ❗️ You don't have to wait until the end of the migration. Proceed to the next steps
6. Switch Report Portal values to Single Bucket `minio.foo=bar` (the link will be there after release 23.2).
7. Install Report Portal Helm chart with new Single Bucket values 
```
helm install my-release
```
8. When the data has been successfully migrated, Job.butch will get the status `Completed`. Uninstall `rp-storage-migration/rpsm-multi-single`
```
helm uninstall rpsm-s3-minio
```