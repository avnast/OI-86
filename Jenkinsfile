pipeline {
  agent any

  environment {
    SETUP_CLUSTER="YES"
    SETUP_WORDPRESS="YES"
    AWS_ACCESS_KEY_ID=credentials("aws-key-id")
    AWS_SECRET_ACCESS_KEY=credentials("aws-key")
    EC2_REGION="us-west-2"
    S3URI="s3://oi-86/k8s"
    CLUSTER_URL="https://avnast_k8s.inkubator.opsworks.io"
    K8S_ADMIN_CERT_CRED="kubeadmin-cert"
  }

  stages {

    stage('kube-aws validate') {
      steps {
        dir('cluster') {
          sh 'kube-aws validate --s3-uri $S3URI'
        }
      }
    }

    stage('kube-aws up') {
      when { environment name: 'SETUP_CLUSTER', value: "YES" }
      steps {
        dir('cluster') {
          sh 'kube-aws up --s3-uri $S3URI'
          // wait for k8s up
          withKubeConfig(credentialsId: $K8S_ADMIN_CERT_CRED_ID, serverUrl: $CLUSTER_URL) {
            retry(60) {
              sleep 10
              sh 'kubectl get nodes'
            }
          }
        }
      }
    }

    stage('Launch example Wordpress') {
      when { environment name: 'SETUP_WORDPRESS', value: "YES" }
      steps {
        withKubeConfig(credentialsId: $K8S_ADMIN_CERT_CRED_ID, serverUrl: $CLUSTER_URL) {
          sh 'kubectl apply -f k8s'
        }
      }
    }

  }

  post {
    always {
      deleteDir()
    }
  }

}

