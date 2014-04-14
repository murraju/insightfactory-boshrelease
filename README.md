# Bosh release for Data Science Infrastructures

This is a [Bosh][1] release for building infrastructures for Data Science. This bosh release builds on the work by [Ferran Rodenas][2]  and [Cloud Foundry's Platform Engineering group][3] - [bosh release for mesos][5]

What's already supported from the [mesos-boshrelease][5]:

* [Airbnb Chronos](http://airbnb.github.io/chronos/): A distributed and fault-tolerant scheduler
* [Mesosphere Marathon](https://github.com/mesosphere/marathon): An Apache Mesos framework for long-running services
* [Apache Cassandra](http://cassandra.apache.org/): An open source distributed database management system designed to handle large amounts of data across many commodity servers, providing high availability with no single point of failure
* [Jenkins](http://jenkins-ci.org/): An extendable open source continuous integration server



This release adds the following capabilities:

## Apache Spark

1. A Standalone HA Spark Cluster (HA using ZooKeeper) using Cloud Storage - S3 in this case.
2. Run Spark with Hadoop HDFS (Apache Hadoop 2.4.0).
3. Run Spark on a Mesos Cluster with HDFS (Apache Hadoop 2.4.0).
4. Run Spark on YARN (Apache Hadoop 2.4.0).

## Mesos

Run Standalone HA Mesos Cluster with HDFS (Apache Hadoop 2.4.0).

## Hadoop

Standalone Hadoop Cluster with YARN (Apache Hadoop 2.4.0) - no Namenode HA.

## Clients

A Client VM for accessing cluster resources. The Client contains Spark, Scalding, Cascalog repos as well as pig and hive.

## Usage

Take a look at the manifests directory for sample deployment manifests. Edit required variables with erb tags.

To build:

1. Run `git clone https://github.com/murraju/insightfactory-boshrelease`
2. `cd insightfactory-boshrelease`
3. Run `bosh create release`
4. Run `bosh upload release`
5. Run `bosh deployment sample_manifest.yml`
6. Run `bosh -n deploy`.
7. Run `bosh vms` to list VMs and IPs.
8. Run `bosh ssh client/0`
9. Run `sudo /var/vcap/jobs/client/bin/setup.sh`
10. ssh -i (bosh pem).pem vcap@(client public IP)



## Create new final release

Create a `config/private.yml` file with the following contents:

``` yaml
---
blobstore:
  s3:
    access_key_id:     ACCESS
    secret_access_key: PRIVATE
    bucke_name: BUCKET
```

```
bosh create release
# To test
git commit -m "updated insightfactory"
bosh create release --final
git commit -m "creating vN release"
git tag vN
git push origin master --tags
```

## Road Map

1. Apache Kafka
2. Apache Storm
3. Apache Accumulo
4. Apache HBase
5. Apache Mahout
6. Additional BDAS components

Monitoring:

1. Sensu
2. Reimmann for metrics


## TODO

1. Clean up - refactor, remove duplication.
2. Wiki
3. Google GCE CPI testing
4. Script for creating dirty blobs when creating dev releases (in absence of S3 access)

## Disclaimer

This is a development release and a work in progress. Please log issues. This release has been tested against AWS EC2 and OpenStack BOSH CPIs (Cloud Provider Interface).

## Copyright and Credits

&copy; 2014, Murali Raju <murali.raju@appliv.com> [@murraju][4]

Based on work originally opensourced by [Ferran Rodenas][2] and [Cloud Foundry's Platform Engineering group][3].

[1]: https://github.com/cloudfoundry/bosh
[2]: https://github.com/frodenas
[3]: https://github.com/cf-platform-eng
[4]: http://twitter.com/murraju
[5]: https://github.com/cf-platform-eng/mesos-boshrelease