# GitOps Examples

## Introduction

This repository contains some example Kubernetes/ArgoCD manifests.
It is useful for testing the functionalities, configurations and syncOptions of ArgoCD.


## Getting started

1. We will be using a [kind](https://kind.sigs.k8s.io/) cluster for our tests. Please follow the [installation instructions](https://kind.sigs.k8s.io/docs/user/quick-start/#installing-from-release-binaries) before continuing.

1. Create a new kind cluster called `kind-gitops-test` for our tests:
    ```bash
    kind create cluster --name kind-gitops-test --kubeconfig ~/.kube/kind-gitops-test
    ```
    ```
    Creating cluster "kind-gitops-test" ...
     ✓ Ensuring node image (kindest/node:v1.26.3) 🖼
     ✓ Preparing nodes 📦
     ✓ Writing configuration 📜
     ✓ Starting control-plane 🕹️
     ✓ Installing CNI 🔌
     ✓ Installing StorageClass 💾
    Set kubectl context to "kind-kind-gitops-test"
    You can now use your cluster with:

    kubectl cluster-info --context kind-kind-gitops-test --kubeconfig ~/.kube/kind-gitops-test

    Have a question, bug, or feature request? Let us know! https://kind.sigs.k8s.io/#community 🙂
    ```

1. Export the `KUBECONFIG` variable in your shell.
    ```bash
    export KUBECONFIG=~/.kube/kind-gitops-test
    ```

1. Install ArgoCD on the kind cluster in a new `argocd` namespace:
    > You can find the latest instructions on the ArgoCD [Getting Started](https://argo-cd.readthedocs.io/en/stable/getting_started/#1-install-argo-cd) documentation page.
    ```bash
    kubectl create namespace argocd
    kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
    ```

1. Get the intial admin secret for your test argocd instance:
    ```bash
    kubectl get secret -n argocd argocd-initial-admin-secret --template='{{ index .data.password | base64decode }}{{ "\n" }}'
    ```

1. Port-forward the argocd-server service to get access to the web-ui/api
    ```bash
    kubectl port-forward svc/argocd-server -n argocd 4443:443
    ```

1. Navigate to `https://localhost:4443/`, ingore the certificate warning and login with user `admin` and the initial admin password.

1. [optional] Navigate to `User info` and Click the `UPDATE PASSWORD` button, change the password and reauthenticate

1. Follow the instructions in the README file of each example.
