hcloud-fip-controller
-------

hcloud-fip-controller is a small controller, to handle floating IP management in a [Kubernetes][k8s-home] cluster on hetzner cloud virtual machines.

The running pods will check the hetzner cloud API every 30 seconds to check if the configured IP Addresses (or subnets in case of IPv6) are assigned to the server, the leader is scheduled to. If not it will update the assignments.
You need to make sure, to have the IP Addresses configured on **every** node for this failover to correctly work, as the controller will not take care of the network configuration of the nodes.

TL;DR;
------

```console
$ helm repo add cbeneke https://cbeneke.github.com/helm-charts
$ helm repo update
$ helm install --name hcloud-fip-controller cbeneke/hcloud-fip-controller
```

Introduction
------------

This chart bootstraps a [hcloud-fip-controller][hcloud-fip-controller-home] installation on
a [Kubernetes][k8s-home] cluster using the [Helm][helm-home] package manager.

Prerequisites
-------------

-  Kubernetes 1.10+

Installing the Chart
--------------------

The chart can be installed as follows:

```console
$ helm repo add cbeneke https://cbeneke.github.com/helm-charts
$ helm repo update
$ helm install --name hcloud-fip-controller cbeneke/hcloud-fip-controller
```

The command deploys hcloud-fip-controller on the Kubernetes cluster. This chart does
not provide a default configuration; hcloud-fip-controller will not act on your
Kubernetes Services until you provide
one. The [configuration](#configuration) section lists various ways to
provide this configuration.

Uninstalling the Chart
----------------------

To uninstall/delete the `hcloud-fip-controller` deployment:

```console
$ helm delete hcloud-fip-controller
```

The command removes all the Kubernetes components associated with the
chart, but will not remove the release metadata from `helm` — this will prevent
you, for example, if you later try to create a release also named `hcloud-fip-controller`). To
fully delete the release and release history, simply [include the `--purge`
flag][helm-usage]:

```console
$ helm delete --purge hcloud-fip-controller
```

Configuration
-------------

See `values.yaml` for configuration notes. Specify each parameter
using the `--set key=value[,key=value]` argument to `helm
install`. For example,

```console
$ helm install --name hcloud-fip-controller \
  --set rbac.create=false \
    cbeneke/hcloud-fip-controller
```

The above command disables the use of RBAC rules.

Alternatively, a YAML file that specifies the values for the above
parameters can be provided while installing the chart. For example,

```console
$ helm install --name hcloud-fip-controller -f values.yaml cbeneke/hcloud-fip-controller
```

By default, this chart does not install a configuration for hcloud-fip-controller, and simply
warns you that you must follow [the configuration instructions on hcloud-fip-controller's
website][hcloud-fip-controller-config] to create an appropriate ConfigMap.

**Please note:** By default, this chart expects a ConfigMap named
'hcloud-fip-controller-config' within the same namespace as the chart is
deployed. _This is different than the hcloud-fip-controller documentation_, which
asks you to create a ConfigMap in the `fip-controller` namespace, with
the name of 'fip-controller-config'.

If you have a more complex configuration and want Helm to manage it for you, you
can provide it in the `config` parameter. The configuration format is
[documented on hcloud-fip-controller's website][hcloud-fip-controller-config].

```console
$ cat values.yaml
configInline:
  hcloud_floating_ips:
  - 10.0.0.1
  hcloud_api_token: secret-token
  lease_duration: 30
  lease_name: production
  log_level: info

$ helm install --name hcloud-fip-controller -f values.yaml cbeneke/hcloud-fip-controller
```

[helm-home]: https://helm.sh
[helm-usage]: https://docs.helm.sh/using_helm/
[k8s-home]: https://kubernetes.io
[hcloud-fip-controller-config]: https://github.com/cbeneke/hcloud-fip-controller/
[hcloud-fip-controller-home]: https://github.com/cbeneke/hcloud-fip-controller/