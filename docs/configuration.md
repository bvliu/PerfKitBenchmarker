# Useful Global Flags

The following are some common flags used when configuring PerfKit Benchmarker.

| Flag               | Notes                                                 |
| ------------------ | ----------------------------------------------------- |
| `--helpmatch=pkb`  | see all global flags                                  |
| `--helpmatch=hpcc` | see all flags associated with the hpcc benchmark. You |
:                    : can substitute any benchmark name to see the          :
:                    : associated flags.                                     :
| `--benchmarks`     | A comma separated list of benchmarks or benchmark     |
:                    : sets to run such as `--benchmarks=iperf,ping` . To    :
:                    : see the full list, run `./pkb.py                      :
:                    : --helpmatch=benchmarks                                :
| `--cloud`          | Cloud where the benchmarks are run. See the table     |
:                    : below for choices.                                    :
| `--machine_type`   | Type of machine to provision if pre-provisioned       |
:                    : machines are not used. Most cloud providers accept    :
:                    : the names of pre-defined provider-specific machine    :
:                    : types (for example, GCP supports                      :
:                    : `--machine_type=n1-standard-8` for a GCE              :
:                    : n1-standard-8 VM). Some cloud providers support YAML  :
:                    : expressions that match the corresponding VM spec      :
:                    : machine_type property in the [YAML                    :
:                    : configs](#configurations-and-configuration-overrides) :
:                    : (for example, GCP supports `--machine_type="{cpus\:   :
:                    : 1, memory\: 4.5GiB}"` for a GCE custom VM with 1 vCPU :
:                    : and 4.5GiB memory). Note that the value provided by   :
:                    : this flag will affect all provisioned machines; users :
:                    : who wish to provision different machine types for     :
:                    : different roles within a single benchmark run should  :
:                    : use the [YAML                                         :
:                    : configs](#configurations-and-configuration-overrides) :
:                    : for finer control.                                    :
| `--zones`          | This flag allows you to override the default zone.    |
:                    : See the table below.                                  :
| `--data_disk_type` | Type of disk to use. Names are provider-specific, but |
:                    : see table below.                                      :

The default cloud is 'GCP', override with the `--cloud` flag. Each cloud has a
default zone which you can override with the `--zones` flag, the flag supports
the same values that the corresponding Cloud CLIs take:

| Cloud name   | Default zone  | Notes                                       |
| ------------ | ------------- | ------------------------------------------- |
| GCP          | us-central1-a |                                             |
| AWS          | us-east-1a    |                                             |
| Azure        | eastus2       |                                             |
| IBMCloud     | us-south-1    |                                             |
| AliCloud     | West US       |                                             |
| DigitalOcean | sfo1          | You must use a zone that supports the       |
:              :               : features 'metadata' (for cloud config) and  :
:              :               : 'private_networking'.                       :
| OpenStack    | nova          |                                             |
| CloudStack   | QC-1          |                                             |
| Rackspace    | IAD           | OnMetal machine-types are available only in |
:              :               : IAD zone                                    :
| Kubernetes   | k8s           |                                             |
| ProfitBricks | AUTO          | Additional zones: ZONE_1, ZONE_2, or ZONE_3 |

Example:

```bash
./pkb.py --cloud=GCP --zones=us-central1-a --benchmarks=iperf,ping
```

The disk type names vary by provider, but the following table summarizes some
useful ones. (Many cloud providers have more disk types beyond these options.)

Cloud name | Network-attached SSD | Network-attached HDD
---------- | -------------------- | --------------------
GCP        | pd-ssd               | pd-standard
AWS        | gp3                  | st1
Azure      | Premium_LRS          | Standard_LRS
Rackspace  | cbs-ssd              | cbs-sata

Also note that `--data_disk_type=local` tells PKB not to allocate a separate
disk, but to use whatever comes with the VM. This is useful with AWS instance
types that come with local SSDs, or with the GCP `--gce_num_local_ssds` flag.

If an instance type comes with more than one disk, PKB uses whichever does *not*
hold the root partition. Specifically, on Azure, PKB always uses `/dev/sdb` as
its scratch device.

## Proxy configuration for VM guests.

If the VM guests do not have direct Internet access in the cloud environment,
you can configure proxy settings through `pkb.py` flags.

To do that simple setup three flags (All urls are in notation ): The flag values
use the same `<protocol>://<server>:<port>` syntax as the corresponding
environment variables, for example `--http_proxy=http://proxy.example.com:8080`
.

| Flag            | Notes                                                   |
| --------------- | ------------------------------------------------------- |
| `--http_proxy`  | Needed for package manager on Guest OS and for some     |
:                 : Perfkit packages                                        :
| `--https_proxy` | Needed for package manager or Ubuntu guest and for from |
:                 : Github downloaded packages                              :
| `--ftp_proxy`   | Needed for some Perfkit packages                        |

# Configurations and Configuration Overrides

Each benchmark now has an independent configuration which is written in YAML.
Users may override this default configuration by providing a configuration. This
allows for much more complex setups than previously possible, including running
benchmarks across clouds.

A benchmark configuration has a somewhat simple structure. It is essentially
just a series of nested dictionaries. At the top level, it contains VM groups.
VM groups are logical groups of homogenous machines. The VM groups hold both a
`vm_spec` and a `disk_spec` which contain the parameters needed to create
members of that group. Here is an example of an expanded configuration:

```yaml
hbase_ycsb:
  vm_groups:
    loaders:
      vm_count: 4
      vm_spec:
        GCP:
          machine_type: n1-standard-1
          image: ubuntu-16-04
          zone: us-central1-c
        AWS:
          machine_type: m3.medium
          image: ami-######
          zone: us-east-1a
        # Other clouds here...
      # This specifies the cloud to use for the group. This allows for
      # benchmark configurations that span clouds.
      cloud: AWS
      # No disk_spec here since these are loaders.
    master:
      vm_count: 1
      cloud: GCP
      vm_spec:
        GCP:
          machine_type:
            cpus: 2
            memory: 10.0GiB
          image: ubuntu-16-04
          zone: us-central1-c
        # Other clouds here...
      disk_count: 1
      disk_spec:
        GCP:
          disk_size: 100
          disk_type: standard
          mount_point: /scratch
        # Other clouds here...
    workers:
      vm_count: 4
      cloud: GCP
      vm_spec:
        GCP:
          machine_type: n1-standard-4
          image: ubuntu-16-04
          zone: us-central1-c
        # Other clouds here...
      disk_count: 1
      disk_spec:
        GCP:
          disk_size: 500
          disk_type: remote_ssd
          mount_point: /scratch
        # Other clouds here...
```

For a complete list of keys for `vm_spec`s and `disk_spec`s see
[`virtual_machine_spec.BaseVmSpec`](https://github.com/GoogleCloudPlatform/PerfKitBenchmarker/blob/master/perfkitbenchmarker/virtual_machine.py)
and
[`disk.BaseDiskSpec`](https://github.com/GoogleCloudPlatform/PerfKitBenchmarker/blob/master/perfkitbenchmarker/disk.py)
and their derived classes.

User configs are applied on top of the existing default config and can be
specified in two ways. The first is by supplying a config file via the
`--benchmark_config_file` flag. The second is by specifying a single setting to
override via the `--config_override` flag.

A user config file only needs to specify the settings which it is intended to
override. For example if the only thing you want to do is change the number of
VMs for the `cluster_boot` benchmark, this config is sufficient:

```yaml
cluster_boot:
  vm_groups:
    default:
      vm_count: 100
```

You can achieve the same effect by specifying the `--config_override` flag. The
value of the flag should be a path within the YAML (with keys delimited by
periods), an equals sign, and finally the new value:

```bash
--config_override=cluster_boot.vm_groups.default.vm_count=100
```

See the section below for how to use static (i.e. pre-provisioned) machines in
your config.
# Specifying Flags in Configuration Files

You can now specify flags in configuration files by using the `flags` key at the
top level in a benchmark config. The expected value is a dictionary mapping flag
names to their new default values. The flags are only defaults; it's still
possible to override them via the command line. It's even possible to specify
conflicting values of the same flag in different benchmarks:

```yaml
iperf:
  flags:
    machine_type: n1-standard-2
    zone: us-central1-b
    iperf_sending_thread_count: 2

netperf:
  flags:
    machine_type: n1-standard-8
```

The new defaults will only apply to the benchmark in which they are specified.

