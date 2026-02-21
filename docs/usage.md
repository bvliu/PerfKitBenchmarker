# Running a Single Benchmark

PerfKit Benchmarker can run benchmarks both on Cloud Providers (GCP, AWS, Azure,
DigitalOcean) as well as any "machine" you can SSH into.

## Example run on GCP

```bash
$ ./pkb.py --project=<GCP project ID> --benchmarks=iperf --machine_type=f1-micro
```

## Example run on AWS

```bash
$ cd PerfKitBenchmarker
$ ./pkb.py --cloud=AWS --benchmarks=iperf --machine_type=t2.micro
```

## Example run on Azure

```bash
$ ./pkb.py --cloud=Azure --machine_type=Standard_A0 --benchmarks=iperf
```

## Example run on IBMCloud

```bash
$ ./pkb.py --cloud=IBMCloud --machine_type=cx2-4x8 --benchmarks=iperf
```

## Example run on AliCloud

```bash
$ ./pkb.py --cloud=AliCloud --machine_type=ecs.s2.large --benchmarks=iperf
```

## Example run on DigitalOcean

```bash
$ ./pkb.py --cloud=DigitalOcean --machine_type=16gb --benchmarks=iperf
```

## Example run on OpenStack

```bash
$ ./pkb.py --cloud=OpenStack --machine_type=m1.medium \
           --openstack_network=private --benchmarks=iperf
```

## Example run on Kubernetes

```bash
$ ./pkb.py --vm_platform=Kubernetes --benchmarks=iperf \
           --kubeconfig=/path/to/kubeconfig --use_k8s_vm_node_selectors=False
```

Note that this requires an existing Kubernetes cluster which you have the
kubeconfig for. To spin up a cluster from scratch, see the
[GKE tutorial](./tutorials/gke_walkthrough).

## Example run on Mesos

```bash
$ ./pkb.py --cloud=Mesos --benchmarks=iperf --marathon_address=localhost:8080
```

## Example run on CloudStack

```bash
./pkb.py --cloud=CloudStack --benchmarks=ping --cs_network_offering=DefaultNetworkOffering
```

## Example run on Rackspace

```bash
$ ./pkb.py --cloud=Rackspace --machine_type=general1-2 --benchmarks=iperf
```

## Example run on ProfitBricks

```bash
$ ./pkb.py --cloud=ProfitBricks --machine_type=Small --benchmarks=iperf
```
# How to Run All Standard Benchmarks

Run with `--benchmarks="standard_set"` and every benchmark in the standard set
will run serially which can take a couple of hours. Additionally, if you don't
specify `--cloud=...`, all benchmarks will run on the Google Cloud Platform.

# How to Run All Benchmarks in a Named Set

Named sets are are groupings of one or more benchmarks in the benchmarking
directory. This feature allows parallel innovation of what is important to
measure in the Cloud, and is defined by the set owner. For example the GoogleSet
is maintained by Google, whereas the StanfordSet is managed by Stanford. Once a
quarter a meeting is held to review all the sets to determine what benchmarks
should be promoted to the `standard_set`. The Standard Set is also reviewed to
see if anything should be removed. To run all benchmarks in a named set, specify
the set name in the benchmarks parameter (e.g., `--benchmarks="standard_set"`).
Sets can be combined with individual benchmarks or other named sets.
# Running selective stages of a benchmark

This procedure demonstrates how to run only selective stages of a benchmark.
This technique can be useful for examining a machine after it has been prepared,
but before the benchmark runs.

This example shows how to provision and prepare the `cluster_boot` benchmark
without actually running the benchmark.

1.  Change to your local version of PKB: `cd $HOME/PerfKitBenchmarker`

1.  Run provision, prepare, and run stages of `cluster_boot`.

    ```
    ./pkb.py --benchmarks=cluster_boot --machine_type=n1-standard-2 --zones=us-central1-f --run_stage=provision,prepare,run
    ```

1.  The output from the console will tell you the run URI for your benchmark.
    Try to ssh into the VM. The machine "Default-0" came from the VM group which
    is specified in the benchmark_config for cluster_boot.

    ```
    ssh -F /tmp/perfkitbenchmarker/runs/<run_uri>/ssh_config default-0
    ```

1.  Now that you have examined the machines, teardown the instances that were
    made and cleanup.

    ```
    ./pkb.py --benchmarks=cluster_boot --run_stage=teardown -run_uri=<run_uri>
    ```

