pipeline {
  agent any 
  environment {
    SETUP_CLUSTER="YES"
    SETUP_WORDPRESS="NO"
    AWS_ACCESS_KEY_ID=credentials("aws-key-id")
    AWS_SECRET_ACCESS_KEY=credentials("aws-key")
    AWS_DEFAULT_REGION="us-west-2"
    AWS_AVAILABILITY_ZONE="us-west-2b"
    S3URI="s3://oi-86/k8s"
    CLUSTER_NAME="avnast-k8s-cluster"
    CLUSTER_DNS="avnast_k8s.inkubator.opsworks.io"
    HOSTED_ZONE_ID="Z3DT1QHQCXWAFD"
    KEY_PAIR="av.nast"
    KMS_KEY_ARN="arn:aws:kms:us-west-2:762510625864:key/3ad2bd66-9fbc-4e6d-9ea8-c6b4c1cc6617"
  }
  options {
    buildDiscarder(logRotator(numToKeepStr: '3'))
    disableConcurrentBuilds()
  }
  stages {

    stage('kube-aws init') {
      when { environment name: "SETUP_CLUSTER", value: "YES"; expression { ! fileExists('cluster/cluster.yaml') } }
      sh 'mkdir -p cluster'
      steps {
        dir('cluster') {
          sh 'kube-aws init --cluster-name=$CLUSTER_NAME --external-dns-name=$CLUSTER_DNS --hosted-zone-id=$HOSTED_ZONE_ID --region=$AWS_DEFAULT_REGION --key-name=$KEY_PAIR --availability-zone=$AWS_AVAILABILITY_ZONE --kms-key-arn=$KMS_KEY_ARN'
        }
      }
    }

    stage('kube-aws render') {
      when { environment name: "SETUP_CLUSTER", value: "YES" }
      steps {
        dir('cluster') {
          sh 'kube-aws render credentials --generate-ca'
          sh 'kube-aws render stack'
        }
      }
    }

    stage('kube-aws validate') {
      when { environment name: "SETUP_CLUSTER", value: "YES" }
      steps {
        dir('cluster') {
          sh 'kube-aws validate --s3-uri $S3URI'
        }
      }
    }

    stage('kube-aws up') {
      when { environment name: "SETUP_CLUSTER", value: "YES" }
      steps {
        dir('cluster') {
          sh 'kube-aws up --s3-uri $S3URI'
          // wait for k8s
          retry(60) {
            sleep 10
            sh 'kubectl --kubeconfig=kubeconfig get nodes'
          }
        }
      }
    }

    stage ('Archive cluster configs') {
      when { environment name: "SETUP_CLUSTER", value: "YES" }
      steps {
        zip zipFile:'$CLUSTER_NAME.zip', dir:'cluster', archive:true
      }
    }

    stage('Apply k8s manifests (Wordpress)') {
      when { environment name: "SETUP_WORDPRESS", value: "YES" }
      steps {
        zip zipFile:'manifests.zip', dir:'manifests', archive:true
        sh 'kubectl --kubeconfig=cluster/kubeconfig apply -f manifests'
      }
    }

  }

}

