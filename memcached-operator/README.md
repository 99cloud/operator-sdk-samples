# Memcached Operator

## Prerequisites

- git
- go version v1.12+
- docker version 1.13.1+

## Build and run the Operator

> Before running the operator, The CRD must be register with apiserver

    oc create -f deploy/crds/cache.example.com_memcacheds_crd.yaml
1. Clone repo and build image

        $ git clone https://github.com/99cloud/operator-sdk-samples.git
        $ git checkout animbus-3.11
        $ cd operator-sdk-samples/memcached-operator
        $ make install
        $ make image
1. Run as Deployment inside the cluster

        $ oc create -f deploy/service_account.yaml
        $ oc create -f deploy/cluster_role.yaml
        $ oc create -f deploy/cluster_role_binding.yaml
    **Note**: To apply the RBAC you need to logged in `system:admin`
1. Verify that the `memcached-operator` Deployment is up and runing

        oc get deployment memcached-operator
        NAME                 DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
        memcached-operator   1         1         1            1           4h
1. Verify that the `memcached-operator` pod is up and running:

        oc get pod -l name=memcached-operator
        NAME                                  READY     STATUS    RESTARTS   AGE
        memcached-operator-8646699d79-6pnz2   1/1       Running   0          2h

## Create a Memcached CR

1. Create the example Memcached CR that was generated at `deploy/crds/cache.example.com_v1alpha1_memcached_cr.yaml`:

        $ cat deploy/crds/cache.example.com_v1alpha1_memcached_cr.yaml
        apiVersion: cache.example.com/v1alpha1
        kind: Memcached
        metadata:
          name: example-memcached
        spec:
          # Add fields here
          size: 3
          image: memcached:1.4.36-alpine

        $ oc apply -f deploy/crds/cache.example.com_v1alpha1_memcached_cr.yaml
1. Ensure that the memcached-operator creates the deployment for the CR

        $ oc get deployment
        NAME                     DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
        memcached-operator       1         1         1            1           2m
        example-memcached        3         3         3            3           1m
1. Check the pods and CR status to confirm the status is updated with the memcached pod names:

        $ oc get pods
        NAME                                  READY     STATUS    RESTARTS   AGE
        example-memcached-6fd7c98d8-7dqdr     1/1       Running   0          1m
        example-memcached-6fd7c98d8-g5k7v     1/1       Running   0          1m
        example-memcached-6fd7c98d8-m7vn7     1/1       Running   0          1m
        memcached-operator-7cc7cfdf86-vvjqk   1/1       Running   0          2m

        $ oc get memcached example-memcached -o yaml
        apiVersion: cache.example.com/v1alpha1
        kind: Memcached
        metadata:
          creationTimestamp: 2019-11-06T02:38:35Z
          generation: 1
          name: example-memcached
          namespace: "123"
          resourceVersion: "8303746"
          selfLink: /apis/cache.example.com/v1alpha1/namespaces/123/memcacheds/example-memcached
          uid: 88081d77-003e-11ea-b754-00163e10e25b
        spec:
          image: memcached:1.4.36-alpine
          size: 3
        status:
          nodes:
          - example-memcached-75cf67cd7b-45tf4
          - example-memcached-75cf67cd7b-7z6sh
          - example-memcached-75cf67cd7b-ppxx7
