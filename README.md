# Kubebuilder-StatefulSet

> This repository contains code for the Multi-Version Cronjob (v1 & v2) implemented by Kubebuilder. It doesn't contain webhook implementation.

This **Kubebuilder StatefulSet Cronjob Implementation** is the modified implementation from the [Kubebuilder Cronjob Tutorial](https://book.kubebuilder.io/cronjob-tutorial/cronjob-tutorial.html) featuring the same file struture. This Operator implementation is done for Cronjob in a way that when we deploy the Operator/Controller by `make deploy`, the Operator/Controller is NOT deployed as a 'Deployment' but rather as a 'StatefulSet'. 