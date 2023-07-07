# ansible-operator
A brief explanation on how to build a kubernetes operator without golang knowledge

## Prerequisites
- operator-sdk CLI - [installation guide](https://sdk.operatorframework.io/docs/building-operators/ansible/installation/)
- A user with cluster-admin permissions
- An accessible image registry

## Demo
Initialize an operator's folder
```shell
mkdir my-custom-operator
cd my-custom-operator
operator-sdk init --plugins=ansible --domain ff.io
```

Create our API

```shell
operator-sdk create api --group deployments --version v1alpha1 --kind Deployment --generate-role
```

The generated operator comprend:
- Deployment CRD
- Deployment CR
- A manager that reconciles the state
- A reconciler, aka ansible role/playbook
- A watches.yaml that connects the resource with role/playbook

roles/deployments/defaults/main.yml

Update the custom resource present on config/samples/deployments_v1alpha1_deployment.yaml

make docker-build docker-push IMG=franzu/nginx-operator:v0.0.1
make deploy IMG=franzu/nginx-operator:v0.0.1

update cr config/samples/deployments_v1alpha1_deployment.yaml

kubectl apply -f config/samples/deployments_v1alpha1_deployment.yaml
