# OI-86

Requirements:
  - Jenkins server with installed git, kube-aws and kubectl

Usage:
  - review Jenkinsfile and adjust environment variables as needed
  - create cluster/cluster.yaml with ```kube-aws init``` OR fill corresponding values in Jenkinsfile and it will be created by pipeline
  - trigger Jenkins build with commit or by hand
  - after successful Jenkins build download arhives with kube-aws output and use it with kubectl/kube-aws
