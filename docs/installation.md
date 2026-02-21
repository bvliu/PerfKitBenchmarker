# Installation and Setup

Before you can run the PerfKit Benchmarker, you need account(s) on the cloud
provider(s) you want to benchmark (see
[providers](perfkitbenchmarker/providers/README.md)). You also need the software
dependencies, which are mostly command line tools and credentials to access your
accounts without a password. The following steps should help you get up and
running with PKB.

## Python 3

The recommended way to install and run PKB is in a virtualenv with the latest
version of Python 3 (at least Python 3.12). Most Linux distributions and recent
Mac OS X versions already have Python 3 installed at `/usr/bin/python3`.

If Python is not installed, you can likely install it using your distribution's
package manager, or see the
[Python Download page](https://www.python.org/downloads/).

<!-- copybara:strip_begin(internal) -->
<!--* pragma: { seclinter_this_is_fine: true } *-->
<!-- copybara:strip_end -->
```
# install pyenv to install python on persistent home directory
curl https://pyenv.run | bash

# add to path
echo 'export PATH="$HOME/.pyenv/bin:$PATH"' >> ~/.bashrc
echo 'eval "$(pyenv init -)"' >> ~/.bashrc
echo 'eval "$(pyenv virtualenv-init -)"' >> ~/.bashrc

# update bashrc
source ~/.bashrc

# install python 3.12 and make default
pyenv install 3.12
pyenv global 3.12
```
<!-- copybara:strip_begin(internal) -->
<!--* pragma: { seclinter_this_is_fine: false } *-->
<!-- copybara:strip_end -->

## Install PerfKit Benchmarker

Download the latest PerfKit Benchmarker release from
[GitHub](http://github.com/GoogleCloudPlatform/PerfKitBenchmarker/releases). You
can also clone the working version with:

```bash
$ cd $HOME
$ git clone https://github.com/GoogleCloudPlatform/PerfKitBenchmarker.git
```

Install Python library dependencies:

```bash
$ pip3 install -r $HOME/PerfKitBenchmarker/requirements.txt
```

You may need to install additional dependencies depending on the cloud provider
you are using. For example, for
[AWS](https://github.com/GoogleCloudPlatform/PerfKitBenchmarker/blob/master/perfkitbenchmarker/providers/aws/requirements.txt):

```bash
$ cd $HOME/PerfKitBenchmarker/perfkitbenchmarker/providers/aws
$ pip3 install -r requirements.txt
```

