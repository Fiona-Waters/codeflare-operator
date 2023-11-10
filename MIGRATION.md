# Guide for Migration from RHODS Codeflare-Operator to RHODS Data Science Cluster

### Pre-requisites
 - Existing installation of Codeflare-Operator installed via OperatorHub.

## Migration Steps
Follow these steps in order to migrate to using the updated RHODS Data Science Cluster:

1. Uninstall ODH operator if you have it installed.
    - In the [OpenShift Console](https://console.redhat.com/openshift/) navigate to Operators -> Installed Operators. Search for Open Data Hub, click on it and choose to uninstall.
2. Uninstall CodeFlare Operator.
     - In the [OpenShift Console](https://console.redhat.com/openshift/) navigate to Operators -> Installed Operators. Search for and locate the version of CodeFlare Operator that you have installed, click on it and choose to uninstall.
3. Install RHODS Version 2.4
    - In the [OpenShift Console](https://console.redhat.com/openshift/) navigate to Operators -> OperatorHub. Search for Red Hat OpenShift Data Science and click on it. Choose version 2.4 and click Install.
    - Subscribe to the operator by creating the following subscription:
    ```bash
        cat <<EOF | oc create -f -
        apiVersion: operators.coreos.com/v1alpha1
        kind: Subscription
        metadata:
          name: opendatahub-operator
          namespace: openshift-operators
        spec:
          channel: fast
          name: opendatahub-operator
          source: community-operators
          sourceNamespace: openshift-marketplace
        EOF
    ```
4. Enable the Data Science Cluster:
    - Navigate to Home -> Search -> Resources and search for the default DataScienceCluster CRD and enable CodeFlare and Ray by marking them as `managementState: Managed`.
5. Remove resources from the previous installation that are no longer needed.
    ```bash
    oc delete crd instascales.codeflare.codeflare.dev mcads.codeflare.codeflare.dev quotasubtrees.ibm.com appwrappers.mcad.ibm.com schedulingspecs.mcad.ibm.com
    oc delete deployment kuberay-operator -n opendatahub
    ```
