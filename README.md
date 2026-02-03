# infra-setup

This repository contains infrastructure setup details for Kubernetes and OpenShift clusters in local environments, as well as Terraform Infrastructure as Code (IaC) for AWS-based demo projects within this GitHub organization.

## Prerequisites

### Local Development

Choose one of the following options for local cluster setup:
- **OpenShift (CRC)**: Follow the [Local OpenShift Cluster Setup instructions](#local-openshift-cluster-setup---codeready-container) below to configure a CodeReady Containers (CRC) cluster.

- **Standard Kubernetes**: Use any of the local cluster solutions you want, for example:
  - [Kind](https://kind.sigs.k8s.io/)
  - [Minikube](https://minikube.sigs.k8s.io/)
  - [Docker Desktop (with integrated Kubernetes)](https://www.docker.com/products/docker-desktop/)

### Cloud Managed Environments

- **AWS EKS**: This repository includes a fully documented Infrastructure as Code implementation in the [eks-iac](./eks-iac) directory, covering everything needed for your EKS deployment.

- **Other Cloud Platforms**: You can also use GKE, AKS, ROSA, or ARO to run all demo and POC applications in this GitHub organization. For cluster setup instructions, please refer to the respective cloud provider's official documentation.



### Required Tools
- Terraform: For Infrastructure as Code (IaC) provisioning
- CLI Tools:
  - kubectl (Kubernetes CLI) or oc (OpenShift CLI)
  - aws (AWS CLI) - for AWS deployments

---

## Local OpenShift Cluster Setup - CodeReady Container

- Prerequisites: First, ensure your system meets the requirements:
  - RAM: Minimum 9 GB (16 GB recommended)
  - CPU: 4 physical CPU cores
  - Disk space: 35 GB free space
  - Operating System: Windows, macOS, or Linux

- Download and Install CRC
  - Option A: Visit the Red Hat OpenShift download page and download CRC for your platform.
    - Webpage: <https://developers.redhat.com/content-gateway/rest/mirror/pub/openshift-v4/clients/crc/latest/>
  - Option B: Optionally for macos you if you know the version you want you can use: <https://developers.redhat.com/content-gateway/file/pub/openshift-v4/clients/crc/2.57.0/crc-macos-installer.pkg>

    ```shell
    # After installation you can run
    crc version
    # Example Output-1:
    CRC version: 2.57.0+ae41f6
    OpenShift version: 4.20.5
    MicroShift version: 4.20.0
     
    # NOTE: If there is a newer version this command will give us the newer version URL and you can update it by re-installing the new package. 
    # Example Output - 2:
    WARN A new version (2.57.0) has been published on https://developers.redhat.com/content-gateway/file/pub/openshift-v4/clients/crc/2.57.0/crc-macos-installer.pkg
    CRC version: 2.51.0+80aa80
    OpenShift version: 4.18.2
    MicroShift version: 4.18.2
    ```

- Create and configure new local OpenShift cluster by running:
    > [!NOTE]
    > Login and download the `pull-secret.txt` file from <https://console.redhat.com/openshift/create/local>

    ```shell
    # One-time setup (first time or after upgrade)
    crc setup

    # Download pull secret from console.redhat.com and save it or you can also provide this file path later when `crc start` command prompts for it.
    crc config set pull-secret-file /path/to/pull-secret.txt

    # Option Resource Configs
    ## Set custom CPU cores (default is 4)
    crc config set cpus 10

    ## Set custom memory in MB (default is 10752)
    crc config set memory 49152

    ## Set disk size in GB (default is 31)
    crc config set disk-size 120

    ## Set user defined kubeadmin password 
    crc config set kubeadmin-password Admin@123 #Note, change this example password.

    ## Set user defined developer password
    crc config set developer-password Dev@123 #Note, change this example password.

    ## Verify or to set other properties
    crc config view
    crc config --help


    # Start existing or create new cluster:
    crc start

    # Set up the oc command in your shell
    eval $(crc oc-env)
    ```

- Use the following command to manage the code ready container - OpenShift cluster for local development.

  ```shell
  ###############################
  # Useful Management Commands
  ###############################
  ## Check cluster status
  crc status

  ## Stop the cluster
  crc stop

  ## Delete the cluster
  crc delete

  ## Start existing or create new cluster:
  crc start

  # Set up the oc command in your shell
  eval $(crc oc-env)

  ## View cluster info
  crc console --credentials

  # Open the console in browser
  crc console

  ## View current configuration
  crc config view

  # Login as admin
  oc login -u kubeadmin https://api.crc.testing:6443
  # Enter the password displayed after 'crc start' or `crc console --credentials`

  # Or login as developer
  oc login -u developer https://api.crc.testing:6443
  ```

---

