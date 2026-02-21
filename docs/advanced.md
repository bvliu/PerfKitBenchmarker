# How to Run Windows Benchmarks

Install all dependencies as above and ensure that smbclient is installed on your
system if you are running on a linux controller:

```bash
$ which smbclient
/usr/bin/smbclient
```

Now you can run Windows benchmarks by running with `--os_type=windows`. Windows
has a different set of benchmarks than Linux does. They can be found under
[`perfkitbenchmarker/windows_benchmarks/`](perfkitbenchmarker/windows_benchmarks).
The target VM OS is Windows Server 2012 R2.

# How to Run Benchmarks with Juju

[Juju](https://jujucharms.com/) is a service orchestration tool that enables you
to quickly model, configure, deploy and manage entire cloud environments.
Supported benchmarks will deploy a Juju-modeled service automatically, with no
extra user configuration required, by specifying the `--os_type=juju` flag.

## Example

```bash
$ ./pkb.py --cloud=AWS --os_type=juju --benchmarks=cassandra_stress
```

## Benchmark support

Benchmark/Package authors need to implement the JujuInstall() method inside
their package. This method deploys, configures, and relates the services to be
benchmarked. Please note that other software installation and configuration
should be bypassed when `FLAGS.os_type == JUJU`. See
[`perfkitbenchmarker/linux_packages/cassandra.py`](perfkitbenchmarker/linux_packages/cassandra.py)
for an example implementation.

# Advanced: How To Run Benchmarks Without Cloud Provisioning (e.g., local workstation)

It is possible to run PerfKit Benchmarker without running the Cloud provisioning
steps. This is useful if you want to run on a local machine, or have a benchmark
like iperf run from an external point to a Cloud VM.

In order to do this you need to make sure:

*   The static (i.e. not provisioned by PerfKit Benchmarker) machine is ssh'able
*   The user PerfKitBenchmarker will login as has 'sudo' access. (*** Note we
    hope to remove this restriction soon ***)

Next, you will want to create a YAML user config file describing how to connect
to the machine as follows:

```yaml
static_vms:
  - &vm1 # Using the & character creates an anchor that we can
         # reference later by using the same name and a * character.
    ip_address: 170.200.60.23
    user_name: voellm
    ssh_private_key: /home/voellm/perfkitkeys/my_key_file.pem
    zone: Siberia
    disk_specs:
      - mount_point: /data_dir
```

*   The `ip_address` is the address where you want benchmarks to run.
*   `ssh_private_key` is where to find the private ssh key.
*   `zone` can be anything you want. It is used when publishing results.
*   `disk_specs` is used by all benchmarks which use disk (i.e., `fio`,
    `bonnie++`, many others).

In the same file, configure any number of benchmarks (in this case just iperf),
and reference the static VM as follows:

```yaml
iperf:
  vm_groups:
    vm_1:
      static_vms:
        - *vm1
```

I called my file `iperf.yaml` and used it to run iperf from Siberia to a GCP VM
in us-central1-f as follows:

```bash
$ ./pkb.py --benchmarks=iperf --machine_type=f1-micro --benchmark_config_file=iperf.yaml --zones=us-central1-f --ip_addresses=EXTERNAL
```

*   `ip_addresses=EXTERNAL` tells PerfKit Benchmarker not to use 10.X (ie
    Internal) machine addresses that all Cloud VMs have. Just use the external
    IP address.

If a benchmark requires two machines like iperf, you can have two machines in
the same YAML file as shown below. This means you can indeed run between two
machines and never provision any VMs in the Cloud.

```yaml
static_vms:
  - &vm1
    ip_address: <ip1>
    user_name: connormccoy
    ssh_private_key: /home/connormccoy/.ssh/google_compute_engine
    internal_ip: 10.240.223.37
    install_packages: false
  - &vm2
    ip_address: <ip2>
    user_name: connormccoy
    ssh_private_key: /home/connormccoy/.ssh/google_compute_engine
    internal_ip: 10.240.234.189
    ssh_port: 2222

iperf:
  vm_groups:
    vm_1:
      static_vms:
        - *vm2
    vm_2:
      static_vms:
        - *vm1
```

# Integration Testing

If you wish to run unit or integration tests, ensure that you have `tox >=
2.0.0` installed.

In addition to regular unit tests, which are run via
[`hooks/check-everything`](hooks/check-everything), PerfKit Benchmarker has
integration tests, which create actual cloud resources and take time and money
to run. For this reason, they will only run when the variable
`PERFKIT_INTEGRATION` is defined in the environment. The command

```bash
$ tox -e integration
```

will run the integration tests. The integration tests depend on having installed
and configured all of the relevant cloud provider SDKs, and will fail if you
have not done so.
