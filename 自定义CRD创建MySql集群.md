# 自定义CRD创建MySql集群

  通过自定义CRD，可以很方便的在K8S集群中创建MySqL高可用集群。



## 创建Secret

``` yaml
# 配置数据库的密码、并加密
apiVersion: v1
kind: Secret
metadata:
  name: my-secret
type: Opaque
data:
  # root password is required to be specified
  ROOT_PASSWORD: bm90LXNvLXNlY3VyZQ==
  # a user name to be created, not required
  USER: dXNlcm5hbWU=
  # a password for user, not required
  PASSWORD: dXNlcnBhc3Nz
  # a name for database that will be created, not required
  DATABASE: dXNlcmRi
```
## 创建和部署一个集群

``` yaml
apiVersion: mysql.presslabs.org/v1alpha1
kind: MysqlCluster # 自定义Kind
metadata:
  name: my-cluster 
spec:
  replicas: 3 # 启动3个Mysql并维护期望。
  secretName: my-secret # 关联上面定义的secret 
```

## 配置参数

|            Field Name            |                                         Description                                         |              Example               |      Default value      |
| -------------------------------- | ------------------------------------------------------------------------------------------- | ---------------------------------- | ----------------------- |
| `replicas`                       | The cluster replicas, how many nodes to deploy.                                             | 2                                  | 1 (a single node)       |
| `secretName`                     | The name of the credentials secret. Should be in the same namespace with the cluster.       | `my-secret`                        | *is required*           |
| `initBucketURL`                  | The S3 URL that the cluster initialization backup is taken from                             | `s3://my-bucket/backup.xbackup.gz` | ""                      |
| `initBucketSecretName`           | The secret that the S3 credentials for the initialization are read from                     | `backups-secret`                   | ""                      |
| `backupSchedule`                 | Represents the time and frequency of making cluster backups, in a cron format with seconds. | `0 0 0 * * *`                      | ""                      |
| `backupURL`                      | The bucket URL where to put the backup.                                                     | `gs://bucket/app`                  | ""                      |
| `backupSecretName`               | The name of the secret that contains credentials for connecting to the storage provider.    | `backups-secret`                   | ""                      |
| `backupScheduleJobsHistoryLimit` | The number of many backups to keep.                                                         | `10`                               | inf                     |
| `image`                          | Specify a custom Docker image to be used for the MySQL server container.                    | `percona:5.7-centos`               | nil                     |
| `mysqlConf`                      | Key-value configs for MySQL that will be set in `my.cnf` under `mysqld` section.            | `max_allowed_packet: 128M`         | {}                      |
| `podSpec`                        | This allows to specify pod-related configs. (e.g. `imagePullSecrets`, `labels` )            |                                    | {}                      |
| `volumeSpec`                     | Specifications for PVC, HostPath or EmptyDir, used to store data.                           |                                    | (a PVC with size = 1GB) |
| `maxSlaveLatency`                | The allowed slave lag until it's removed from read service. (in seconds)                    | `30`                               | nil                     |
| `queryLimits`                    | Parameters for pt-kill to ensure some query run limits. (e.g. idle time)                    | `idleTime: 60`                     | nil                     |
| `readOnly`                       | A Boolean value that sets the cluster in read-only state.                                   | `True`                             | False                   |

