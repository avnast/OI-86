pipeline {
  agent any 
  environment {
    S3URI="s3://oi-86/k8s"
  }
  stages {

    stage('kube-aws') {
      steps {
        sh 'cd cluster'
        sh 'kube-aws render credentials --generate-ca'
        sh 'kube-aws render stack'
        sh 'kube-aws validate --s3-uri $S3URI'
        sh 'kube-aws up --s3-uri $S3URI'
      }
    }

    stage('wait for k8s') {
      steps {
        retry(60) {
          sleep 10;
          sh 'kubectl --kubeconfig=kubeconfig get nodes';
        }
      }
    }

    stage('Apply k8s manifests') {
      steps {
        sh 'kubectl --kubeconfig=kubeconfig apply -f ../k8s'
      }
    }

  }
}

