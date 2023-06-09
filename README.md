# Report Portal storage migration

Report Portal storage migration:

1. From multiple buckets to single bucket for S3 and MinIO.
2. From single bucket MinIO to single bucket S3.

# Prerequisites
* Kubernetes 1.19+
* Helm 3.2.0+

# Installing the Chart

To deploy the Report Portal storage migration from multiple buckets to single bucket for S3 and MinIO:

```
helm package rpsm-multi-single
helm install my-release rpsm-multi-single-0.1.0-DEV.tgz
```

To deploy Report Portal storage migration from single bucket MinIO to single bucket S3

```
helm package rpsm-s3-minio
helm install my-release rpsm-s3-minio-0.1.0-DEV.tgz
```

# Storage Migration guide

1. Uninstall Report Portal Helm chart `helm uninstall my-release`
2. Package Helm charts
```
helm package rpsm-s3-minio
helm package rpsm-multi-single
```
3. To migrate data from multiple buckets to single bucket deploy `storage-migration/rpsm-multi-single` Helm chart. Use [values.yaml](https://github.com/reportportal/storage-migration/blob/develop/rpsm-multi-single/values.yaml) to pass your values. [More info](https://github.com/reportportal/storage-migration/blob/develop/rpsm-multi-single/README.md)
> ❗️ Data migration depending on the size of your storage (~14Gb per hour).
```
helm install rpsm-multi-single -f values.yaml rpsm-multi-single-0.1.0-DEV.tgz
```
> ❗️ You need to wait until the end of the migration
3. When the data has been successfully migrated, Job.butch will get the status `Completed`. Uninstall `storage-migration/rpsm-multi-single`
```
helm uninstall rpsm-multi-single
```
4. Create a bucket in Amazon S3. [Create your first S3 bucket](https://docs.aws.amazon.com/AmazonS3/latest/userguide/creating-bucket.html)
5. To migrate data from single bucket MinIO to single bucket S3 deploy `storage-migration/rpsm-s3-minio` Helm chart. Use [values.yaml](https://github.com/reportportal/storage-migration/blob/develop/rpsm-s3-minio/values.yaml) to pass your values. [More info](https://github.com/reportportal/storage-migration/blob/develop/rpsm-s3-minio/README.md).
```
helm install rpsm-s3-minio -f values.yaml rpsm-s3-minio-0.1.0-DEV.tgz
```
> ❗️ You don't have to wait until the end of the migration. Proceed to the next steps
6. Switch Report Portal values to Single Bucket `minio.foo=bar` (the link will be there after release 23.2).
7. Install Report Portal Helm chart with new Single Bucket values 
```
helm install my-release
```
8. When the data has been successfully migrated, Job.butch will get the status `Completed`. Uninstall `storage-migration/rpsm-multi-single`
```
helm uninstall rpsm-s3-minio
```
