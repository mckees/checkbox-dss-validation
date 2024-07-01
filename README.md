# Welcome to the Checkbox DSS project!

This repository contains the Checkbox DSS Provider (DSS-specific test cases and test plans for [Checkbox]) as well as everything that is required to build the [checkbox-dss-validation] snap in the snapstore.

# Checkbox DSS Provider

Located in the `checkbox-provider-dss` directory, it contains:

- the test cases (also called "jobs" in the Checkbox jargon) and test plans to be run by Checkbox (in the `units` directory)

# Requirements

- Ubuntu Jammy or Noble (22.04/24.04)
- Supported hardware platforms:
  - Intel platforms with recent GPU (>= Broadwell)

# Installation

Install the Checkbox runtime and build/install the dss provider snaps:

```shell
sudo snap install --classic snapcraft
sudo snap install checkbox22
lxd init --auto
git clone https://github.com/canonical/checkbox-dss-validation
cd checkbox-dss-validation
snapcraft
sudo snap install --dangerous --classic ./checkbox-dss_2.0_amd64.snap
```

Make sure that the provider service is running and active:

```shell
systemctl status snap.checkbox-dss.remote-slave.service
```

# Install dependencies

Some test need dependencies, so in order to run all tests, you might way to install those dependencies.
A helper script is available to install them:

```shell
checkbox-dss.install-dss-deps
```

# Automated Run

To run the test plans:

```shell
checkbox-dss.gpu-plugin-validation
```

# Cleanup

WARNING: The following steps will remove kubectl and microk8s from your machine. If you wish to keep them, do not run.

To clean up and uninstall all installed tests, run:
```shell
checkbox-dss.remove-dss-deps
```

# Develop the Checkbox DSS provider

Since snaps are immutable, it is not possible to modify the content of the scripts or the test cases. Fortunately, Checkbox provides a functionality to side-load a provider on the DUT.

Therefore, if you want to edit a job definition, a script or a test plan, run the following commands on the DUT:

```shell
cd $HOME
git clone https://github.com/mckees/checkbox-dss-validation
mkdir /var/tmp/checkbox-providers
cp -r $HOME/checkbox-dss-validation/checkbox-provider-dss /var/tmp/checkbox-providers/
```

You can then modify the content of the provider in `/var/tmp/checkbox-providers/checkbox-provider-dss/`, and it's this version that will be used when you run the tests.

Please refer to the [Checkbox documentation] on side-loading providers for more information.

[Checkbox]: https://checkbox.readthedocs.io/
[Checkbox documentation]: https://checkbox.readthedocs.io/en/latest/side-loading.html
