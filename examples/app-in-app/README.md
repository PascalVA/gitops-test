# app-in-app example

1. Follow the instructions on the main [README](/README.md) to setup a temporary argocd test cluster using kind.

1. Make sure you are logged in to the argocd test instance on the cli:
    ```shell
    # port forward the server to your localhost
    kubectl port-forward svc/argocd-server -n argocd 4443:443

    # login with argocd cli
    argocd login localhost:4443
    ```

1. Create the kubernetes namespaces `customer01` and `customer02` on our argocd-test (kind) cluster:
    ```shell
    kubectl create namespace customer01
    kubectl create namespace customer02
    ```

1. Add our github repository on argocd:
    ```shell
    argocd repo add --name gitops-test https://github.com/pascalva/gitops-test
    ```

1. Create the `customers`, `customers-customer01` and `customers-customer02` **projects** and their **root applications**:
    ```shell
    # you can add the --upsert flag to the commands below
    # if you want to overwrite an existing object

    # create the customers root project
    argocd proj create -f examples/app-in-app/rootproject.yml

    # create the customer01 project
    argocd proj create -f examples/app-in-app/customer01/appproject.yml

    # create the customer01 root application
    argocd app create -f examples/app-in-app/customer01/rootapplication.yml

    # create the customer02 project
    argocd proj create -f examples/app-in-app/customer02/appproject.yml

    # create the customer02 root application
    argocd app create -f examples/app-in-app/customer02/rootapplication.yml
    ```

1. You will now have a test environment setup using the app-in app pattern with a static application and an applicationset that generates 3 applications for each customer
