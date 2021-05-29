# Databases

## MariaDB (MySQL)

<!-- markdownlint-disable DOCSMD004 -->
::: details Compatibility & Differences to MySQL
Information on the compatibility of MariaDB software with similar, competing software, as well as the differences, can be found [here](https://mariadb.com/kb/en/compatibility-differences).
:::
<!-- markdownlint-enable DOCSMD004 -->

The following image shows a simplified schema of a standalone Zerops MariaDB Service in HA mode (with default 3 database instances) with no access from the external environment. It means access from outside of Zerops project infrastructure, like access from the Internet. There is only native access through a private network using a default port 3306 from the Zerops Project Basic Service itself, representing the core of a user-defined project inside Zerops.

![Zerops MariaDB Service](./images/Zerops-MariaDB-Service-Base.png "Zerops MariaDB Service")

Zerops Project Basic Service is the hearth of each Zerops project. It has its own [pricing logic](/documentation/overview/pricing.html#projects). The essential parts are two running instances of the **project balancer** (one at the active state and the other at standby backup state) through which all communication is passing (either related to the project's external environment or the private network), which ensures a high degree of reliability and stability for all traffic at any time. Each of them runs in a different container located on a **different physical machine**. An independent **activity controller** continuously monitors critical operating parameters of both project balancers. If the current active instance has any abnormalities, it activates the running standby backup instead. From an external perspective, this change is not noticeable in any way.

Zerops MariaDB Service shares the [same price structure](/documentation/overview/pricing.html#services) with other Zerops services. Here again, two instances of the **[application balancer](https://mariadb.com/kb/en/maxscale)** (both at the active state) play a crucial role and parallelly control the routing of data reading requests, always to the least busy instance of the MariaDB database service or data writing requests to the primary one. The independent scaling controller monitors and controls [vertical scaling](/documentation/automatic-scaling/how-automatic-scaling-works.html#vertical-scaling) (vCPU, RAM, Disk) for all application balancer and MariaDB database containers and [horizontal scaling](/documentation/automatic-scaling/how-automatic-scaling-works.html#horizontal-scaling) (number of containers) for MariaDB database instances. The independent repair controller is then responsible for removing those containers that exhibit abnormal behavior and replacing them with new ones.

Both application balancers or MariaDB service containers are located on **different physical computers** to prevent service outages in the event of any fatal failure on a single physical computer.

![Zerops MariaDB Service](./images/Zerops-MariaDB-Service-Detail.png "Zerops MariaDB Service")

<!-- markdownlint-disable DOCSMD004 -->
::: details Primary & Replica MariaDB instances
There is always only one primary instance with write access. The reason is to minimize the risk for data drift and errant transactions.
:::
<!-- markdownlint-enable DOCSMD004 -->

<!-- markdownlint-disable DOCSMD004 -->
::: warning Volatile non-database storage
Each container has a separate disk space, which can be used using the appropriate API of the database service and thus store data outside the replicated contents of the database. It should be noted that such data is considered volatile (reserved for this particular instance only). It will not be migrated if such a container is deleted, regardless of horizontal scaling or due to a failure of this container. If it is necessary to store and share such data at the database level permanently, then developers should use Zerops object storage or shared storage services in such cases.
:::
<!-- markdownlint-enable DOCSMD004 -->

## MongoDB

## Redis

## Elasticsearch

## RabbitMQ
