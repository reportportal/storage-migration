# Report Portal storage migration
Report Portal storage migration from single bucket MinIO to single bucket S3

# Parameters
## Image Parameters
|Name|Description|Value|
|-|-|-|
|`image.repository`|Image repository|`reportportal/complex-migrations`|
|`image.tag`|Image tag|`1.0.0`|
|`image.pullPolicy`|Image pull policy|`IfNotPresent`|
|`imagePullSecrets`|Specify docker-registry secret names as an array|`[]`|

## Common parameters
|Name|Description|Value|
|-|-|-|
|`nameOverride`|String to partially override rabbitmq.fullname template (will maintain the release name)|`""`|
|`fullnameOverride`|String to fully override rabbitmq.fullname template|`""`|

## Job.batch parameters
|Name|Description|Value|
|-|-|-|
|`podAnnotations`|Pod annotations. Evaluated as a template|`{}`|
|`podSecurityContext`|Security Context|`{}`|
|`securityContext`|Container Security Context|`{}`|
|`resources.limits`|The limits limits for container|`{}`|
|`resources.requests`|The resources limits for container|`{}`|
|`nodeSelector`|Node labels for pod assignment. Evaluated as a template|`{}`|
|`tolerations`|Tolerations for pod assignment. Evaluated as a template|`[]`|
|`affinity`|Pod affinity|`[]`|

## Container environments
|Name|Description|Value|
|-|-|-|
|`storage.minio.secretName`|Secret name for Job.batch with access and secret key to MinIO|`""`|
|`storage.minio.accesskey`|Access key to MinIO. Required if `SecretName` is not specified|`""`|
|`storage.minio.secretkey`|Secret key to MinIO. Required if `SecretName` is not specified|`""`|
|`storage.minio.accesskeyName`|Key form K8s Secret for Access key (`.data.access-key`)|`access-key`|
|`storage.minio.secretkeyName`|Key form K8s Secret for Secret key (`.data.secret-key`)|`secret-key`|
|`storage.minio.endpoint`|MinIO endpoint|`http://<minio-release-name>-minio.default.svc.cluster.local:9000`|
|`storage.s3.secretName`|Secret name for Job.batch with access and secret key to S3|`""`|
|`storage.s3.accesskey`|Access key to S3. Required if `SecretName` is not specified|`""`|
|`storage.s3.secretkey`|Secret key to S3. Required if `SecretName` is not specified|`""`|
|`storage.s3.accesskeyName`|Key form K8s Secret for Access key (`.data.access-key`)|`access-key`|
|`storage.s3.secretkeyName`|Key form K8s Secret for Secret key (`.data.secret-key`)|`secret-key`|
|`storage.s3.endpoint`|S3 endpoint. Ref: https://docs.aws.amazon.com/general/latest/gr/s3.html|`http://s3.eu-central-1.amazonaws.com`|
|`storage.bucket.fromMinioBucket`|MinIO bucket from which data will migrate|`rp-minio-storage`|
|`storage.bucket.toS3Bucket`|S3 bucket where data will be migrated to|`rp-s3-storage`|
