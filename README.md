# CAPI Update playbooks

There is a specific order to run these playbooks to ensure that your cluster is correctly updated. Doing these out of order my result in a broken or unusable CAPI cluster.

The order that we run these playbooks as follows

```
ansible-playbook capi-update-machinetemplates.yml \
  -e kubeconfig_path=/home/kanderson/.kube/config-nesi-capi-test-mgmt \
  -e cluster_name=nesi-capi-test-mgmt \
  -e kubernetes_version=v1.32.7

ansible-playbook capi-update-kubeadmcontrolplane.yml \
  -e kubeconfig_path=/home/kanderson/.kube/config-nesi-capi-test-mgmt \
  -e cluster_name=nesi-capi-test-mgmt \
  -e kubernetes_version=v1.32.7

ansible-playbook capi-update-machinedeployment.yml \
  -e kubeconfig_path=/home/kanderson/.kube/config-nesi-capi-test-mgmt \
  -e cluster_name=nesi-capi-test-mgmt \
  -e kubernetes_version=v1.32.7

ansible-playbook capi-cleanup-machinetemplates.yml \
  -e kubeconfig_path=/home/kanderson/.kube/config-nesi-capi-test-mgmt \
  -e cluster_name=nesi-capi-test-mgmt \
  -e kubernetes_version=v1.32.7
```

To ensure that each one is run for your specified cluster we have no deafult variables set and all must be set during the run of the playbook

```
ansible-playbook EXAMPLE_PLAYBOOK.yml \
  -e kubeconfig_path=/home/kanderson/.kube/config-nesi-capi-test-mgmt \
  -e cluster_name=nesi-capi-test-mgmt \
  -e kubernetes_version=v1.32.7
```

The variables are as follows
```
kubeconfig_path    - This is the management cluster kubeconfig file location
cluster_name       - The name of the cluster that you are wanting to update
kubernetes_version - The kubernetes version that you are upgrading too, you need to ensure that this starts with v EG v1.32.7
```

Here is an easy code/pasta of the variables to change/update with the include line break
```
  -e kubeconfig_path=KUBE_CONFIG_LOCATION \
  -e cluster_name=CLUSTER_TO_UPGRADE \
  -e kubernetes_version=KUBERNETES_VERSION_TO_UPGRADE_TOO
```

By default we have a setting called `dry_run` and this is set to true by default setting this to false will run the apply for the config files
```
-e dry_run=false
```

## Requirements

You will need to run this to add `jmespath` to the pipx venv for ansible

```
pipx inject ansible-core jmespath
```