# Installation

!!! note
    Currently this project is tightly coupled to PGO for data storage and Zitadel as it's identity provider. This is not intended to be the end goal but is the current state for now to get working quickly.

## Prerequisites

Before installing the operator you will need the following

* Kubernetes cluster with OLM installed
* PGO installed
* Zitadel installed

### Install Operator Lifecycle Manager (OLM)

To install OLM, I'm using the operator sdk which is bundled into the devcontainer:

```bash
operator-sdk olm install
```

### Install PGO

Postgres Operator will serve as storage for Keycloak and will be one of the providers that we use to store tenant data. It allows you to create production ready Postgres Clusters.

```bash
kubectl create -f https://operatorhub.io/install/postgresql.yaml
```

Out the box this allows you to configure postgres clusters in any namespace.

### Install Zitadel

First setup a database with PGO:

=== "Database"

    ```bash
    kubectl create ns zitadel
    kubectl apply -f - << EOF
    apiVersion: postgres-operator.crunchydata.com/v1beta1
    kind: PostgresCluster
    metadata:
      name: zitadel-db
      namespace: zitadel
    spec:
      postgresVersion: 18
      instances:
        - name: instance1
          dataVolumeClaimSpec:
            accessModes:
            - "ReadWriteOnce"
            resources:
              requests:
                storage: 1Gi
      users:
        - name: zitadel
          databases:
            - zitadel
        - name: admin
          options: 'SUPERUSER'
    EOF
    ```

=== "Keycloak"

 