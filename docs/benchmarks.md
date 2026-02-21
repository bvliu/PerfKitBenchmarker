# Licensing

PerfKit Benchmarker provides wrappers and workload definitions around popular
benchmark tools. We made it very simple to use and automate everything we can.
It instantiates VMs on the Cloud provider of your choice, automatically installs
benchmarks, and runs the workloads without user interaction.

Due to the level of automation you will not see prompts for software installed
as part of a benchmark run. Therefore you must accept the license of each of the
benchmarks individually, and take responsibility for using them before you use
the PerfKit Benchmarker.

Moving forward, you will need to run PKB with the explicit flag
--accept-licenses.

In its current release these are the benchmarks that are executed:

-   `aerospike`:
    [Apache v2 for the client](http://www.aerospike.com/aerospike-licensing/)
    and
    [GNU AGPL v3.0 for the server](https://github.com/aerospike/aerospike-server/blob/master/LICENSE)
-   `bonnie++`: [GPL v2](http://www.coker.com.au/bonnie++/readme.html)
-   `cassandra_ycsb`: [Apache v2](http://cassandra.apache.org/)
-   `cassandra_stress`: [Apache v2](http://cassandra.apache.org/)
-   `cloudsuite3.0`:
    [CloudSuite 3.0 license](http://cloudsuite.ch/pages/license/)
-   `cluster_boot`: MIT License
-   `coremark`: [EEMBC](https://www.eembc.org/)
-   `copy_throughput`: Apache v2
-   `fio`: [GPL v2](https://github.com/axboe/fio/blob/master/COPYING)
-   [`gpu_pcie_bandwidth`](https://developer.nvidia.com/cuda-downloads):
    [NVIDIA Software Licence Agreement](http://docs.nvidia.com/cuda/eula/index.html#nvidia-driver-license)
-   `hadoop_terasort`: [Apache v2](http://hadoop.apache.org/)
-   `hpcc`: [Original BSD license](http://icl.cs.utk.edu/hpcc/faq/#263)
-   [`hpcg`](https://github.com/hpcg-benchmark/hpcg/):
    [BSD 3-clause](https://github.com/hpcg-benchmark/hpcg/blob/master/LICENSE)
-   `iperf`:
    [UIUC License](https://sourceforge.net/p/iperf2/code/ci/master/tree/doc/ui_license.html)
-   `memtier_benchmark`:
    [GPL v2](https://github.com/RedisLabs/memtier_benchmark)
-   `mesh_network`:
    [HP license](http://www.calculate-linux.org/packages/licenses/netperf)
-   `mongodb`: **Deprecated**.
    [GNU AGPL v3.0](http://www.mongodb.org/about/licensing/)
-   `mongodb_ycsb`: [GNU AGPL v3.0](http://www.mongodb.org/about/licensing/)
-   [`multichase`](https://github.com/google/multichase):
    [Apache v2](https://github.com/google/multichase/blob/master/LICENSE)
-   `netperf`:
    [HP license](http://www.calculate-linux.org/packages/licenses/netperf)
-   [`oldisim`](https://github.com/GoogleCloudPlatform/oldisim):
    [Apache v2](https://github.com/GoogleCloudPlatform/oldisim/blob/master/LICENSE.txt)
-   `object_storage_service`: Apache v2
-   `pgbench`: [PostgreSQL Licence](https://www.postgresql.org/about/licence/)
-   `ping`: No license needed.
-   `silo`: MIT License
-   `scimark2`: [public domain](http://math.nist.gov/scimark2/credits.html)
-   `speccpu2006`: [SPEC CPU2006](http://www.spec.org/cpu2006/)
-   [`SHOC`](https://github.com/vetter/shoc):
    [BSD 3-clause](https://github.com/vetter/shoc/blob/master/LICENSE.md)
-   `sysbench_oltp`: [GPL v2](https://github.com/akopytov/sysbench)
-   [`TensorFlow`](https://github.com/tensorflow/tensorflow):
    [Apache v2](https://github.com/tensorflow/tensorflow/blob/master/LICENSE)
-   [`tomcat`](https://github.com/apache/tomcat):
    [Apache v2](https://github.com/apache/tomcat/blob/trunk/LICENSE)
-   [`unixbench`](https://github.com/kdlucas/byte-unixbench):
    [GPL v2](https://github.com/kdlucas/byte-unixbench/blob/master/LICENSE.txt)
-   [`wrk`](https://github.com/wg/wrk):
    [Modified Apache v2](https://github.com/wg/wrk/blob/master/LICENSE)
-   [`ycsb`](https://github.com/brianfrankcooper/YCSB) (used by `mongodb`,
    `hbase_ycsb`, and others):
    [Apache v2](https://github.com/brianfrankcooper/YCSB/blob/master/LICENSE.txt)

Some of the benchmarks invoked require Java. You must also agree with the
following license:

-   `openjdk-7-jre`:
    [GPL v2 with the Classpath Exception](http://openjdk.java.net/legal/gplv2+ce.html)

[SPEC CPU2006](https://www.spec.org/cpu2006/) benchmark setup cannot be
automated. SPEC requires that users purchase a license and agree with their
terms and conditions. PerfKit Benchmarker users must manually download
`cpu2006-1.2.iso` from the SPEC website, save it under the
`perfkitbenchmarker/data` folder (e.g.
`~/PerfKitBenchmarker/perfkitbenchmarker/data/cpu2006-1.2.iso`), and also supply
a runspec cfg file (e.g.
`~/PerfKitBenchmarker/perfkitbenchmarker/data/linux64-x64-gcc47.cfg`).
Alternately, PerfKit Benchmarker can accept a tar file that can be generated
with the following steps:

*   Extract the contents of `cpu2006-1.2.iso` into a directory named `cpu2006`
*   Run `cpu2006/install.sh`
*   Copy the cfg file into `cpu2006/config`
*   Create a tar file containing the `cpu2006` directory, and place it under the
    `perfkitbenchmarker/data` folder (e.g.
    `~/PerfKitBenchmarker/perfkitbenchmarker/data/cpu2006v1.2.tgz`).

PerfKit Benchmarker will use the tar file if it is present. Otherwise, it will
search for the iso and cfg files.

## Preprovisioned Data

As mentioned above, some benchmarks require preprovisioned data. This section
describes how to preprovision this data.

### Benchmarks with Preprovisioned Data

#### Sample Preprovision Benchmark

This benchmark demonstrates the use of preprovisioned data. Create the following
file to upload using the command line:

```bash
echo "1234567890" > preprovisioned_data.txt
```

To upload, follow the instructions below with a filename of
`preprovisioned_data.txt` and a benchmark name of `sample`.

### Clouds with Preprovisioned Data

#### Google Cloud

To preprovision data on Google Cloud, you will need to upload each file to
Google Cloud Storage using gsutil. First, you will need to create a storage
bucket that is accessible from VMs created in Google Cloud by PKB. Then copy
each file to this bucket using the command

```bash
gsutil cp <filename> gs://<bucket>/<benchmark-name>/<filename>
```

To run a benchmark on Google Cloud that uses the preprovisioned data, use the
flag `--gcp_preprovisioned_data_bucket=<bucket>`.

#### AWS

To preprovision data on AWS, you will need to upload each file to S3 using the
AWS CLI. First, you will need to create a storage bucket that is accessible from
VMs created in AWS by PKB. Then copy each file to this bucket using the command

```bash
aws s3 cp <filename> s3://<bucket>/<benchmark-name>/<filename>
```

To run a benchmark on AWS that uses the preprovisioned data, use the flag
`--aws_preprovisioned_data_bucket=<bucket>`.

