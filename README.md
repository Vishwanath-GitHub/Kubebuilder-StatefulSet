# Kubebuilder-StatefulSet

> This repository contains code for the Multi-Version Cronjob (v1 & v2) implemented by Kubebuilder. It doesn't contain webhook implementation.

This **Kubebuilder StatefulSet Cronjob Implementation** is the modified implementation from the [Kubebuilder Cronjob Tutorial](https://book.kubebuilder.io/cronjob-tutorial/cronjob-tutorial.html) featuring the same file struture. This Operator implementation is done for Cronjob in a way that when we deploy the Operator/Controller by `make deploy`, the Operator/Controller is NOT deployed as a 'Deployment' but rather as a 'StatefulSet'.

Changes made for replacing Deployment with StatefulSet:

* In the file "manager.yaml" inside "config/manager", the field `kind: Deployment` is replaced with `kind: StatefulSet`.
* A Headless-Service is added with this StatefulSet which actually is an 'nginx' service. It is responsible for handling the connection and data transfers between the pods that are deployed by StatefulSet. Also, it is important to deploy the service before deploying the StatefulSet. In case if the service is not integrated with the StatefulSet, then the communication between the pods occurs through the `kube-dns` directly. You can read about this here : [StatefulSet without Service](https://github.com/kubernetes/kubernetes/issues/69608#issuecomment-558813230).

* A field `serviceName:nginx` is added to the StatefulSet part of the "manager.yaml" file. This is extremely important. This tells to the StatefulSet that it has to maintain the connection between the pods deployed by itself with the name of the service provided. In case if the service is not integrated with the StatefulSet, then any name can be provided with the `serviceName:` field. It will not matter since the communication between the pods will occur through the `kube-dns` directly.
* In the file "manager_auth_proxy_patch.yaml" inside "config/default", the field `kind: Deployment` is replaced with `kind: StatefulSet`. This is done so that this patch will perform the RBAC authorization and tell Kubernetes that it has to register a StatefulSet.

After this, the entire Operator/Controller with its configuration is deployed using `make install` and `make run` locally (outside the cluster). To deploy it inside the cluster, use `make docker-build` and `make deploy`. After deploying the controller, it starts watching the events from the workload and shows it by logs of the Operator/Controller StatefulSet pod. Now, when the .yaml of Cronjob is applied, the logs start updating with the status about and from the Cronjob and now this Operator\Controller will manage and monitor this Cronjob.

However, Webhooks are NOT implemented in this Operator/Controller configuration due to the error "Undefined v1.beta1.Webhook" delivered by "parser.go".