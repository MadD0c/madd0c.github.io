# MariaDB

[MariaDB](https://mariadb.org) is a community-developed fork of the MySQL relational database management system intended to remain free under the GNU GPL. Being a fork of a leading open source software system, it is notable for being led by the original developers of MySQL, who forked it due to concerns over its acquisition by Oracle. Contributors are required to share their copyright with the MariaDB Foundation.
The intent is also to maintain high compatibility with MySQL, ensuring a "drop-in" replacement capability with library binary equivalency and exact matching with MySQL APIs and commands. It includes the XtraDB storage engine for replacing InnoDB, as well as a new storage engine, Aria, that intends to be both a transactional and non-transactional engine perhaps even included in future versions of MySQL.

> [wikipedia.org/wiki/MariaDB](https://en.wikipedia.org/wiki/MariaDB)

<img src="https://raw.githubusercontent.com/docker-library/docs/74e3b3d4d60389208732dbd2c95145868111d959/mariadb/logo.png" alt="logo">

This chart is based on the Bitnami Helm Chart but has been adapted to use the Official Docker Image. This allows it to be used on all architectures unlike the Bitnami imabe which is amd64 only.

## TL;DR

```bash
$ helm repo add bitnami https://madd0c.github.io/charts
$ helm install my-release madd0c/mariadb
```
## Prerequisites

- Kubernetes 1.12+
- Helm 2.12+ or Helm 3.0-beta3+
- PV provisioner support in the underlying infrastructure

## Installing the Chart

To install the chart with the release name `my-release`:

```bash
$ helm install my-release madd0c/mariadb
```

The command deploys MariaDB on the Kubernetes cluster in the default configuration. The [Parameters](#parameters) section lists the parameters that can be configured during installation.

> **Tip**: List all releases using `helm list`

## Uninstalling the Chart

To uninstall/delete the `my-release` deployment:

```bash
$ helm delete my-release
```

The command removes all the Kubernetes components associated with the chart and deletes the release.


## Environment Variables
When you install the chart, you can adjust the configuration of the MariaDB instance by passing one or more environment variables on via the helm CLI or in a values.yml. Do note that none of the variables below will have any effect if you start the service with a data directory that already contains a database: any pre-existing database will always be left untouched on container startup.

### MYSQL_ROOT_PASSWORD
This variable is mandatory and specifies the password that will be set for the MariaDB root superuser account.

### MYSQL_DATABASE
This variable is optional and allows you to specify the name of a database to be created on image startup. If a user/password was supplied (see below) then that user will be granted superuser access (corresponding to GRANT ALL) to this database.

### MYSQL_USER, MYSQL_PASSWORD
These variables are optional, used in conjunction to create a new user and to set that user's password. This user will be granted superuser permissions (see above) for the database specified by the MYSQL_DATABASE variable. Both variables are required for a user to be created.

Do note that there is no need to use this mechanism to create the root superuser, that user gets created by default with the password specified by the MYSQL_ROOT_PASSWORD variable.

## Persistence
The MariaDB image stores the MariaDB data and configurations at the /var/lib/mysql path of the container.

The chart mounts a Persistent Volume volume at this location. The volume is created using dynamic volume provisioning, by default. An existing PersistentVolumeClaim can be defined.

Adjust permissions of persistent volume mountpoint
As the image run as non-root by default, it is necessary to adjust the ownership of the persistent volume so that the container can write data into it.

By default, the chart is configured to use Kubernetes Security Context to automatically change the ownership of the volume. However, this feature does not work in all Kubernetes distributions. As an alternative, this chart supports using an initContainer to change the ownership of the volume before mounting it in the final destination.

You can enable this initContainer by setting volumePermissions.enabled to true.
