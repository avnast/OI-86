pipeline {
  agent any 
  environment {
    AWS_ACCESS_KEY_ID=credentials("aws-key-id")
    AWS_ACCESS_KEY=credentials("aws-key")
    EC2_REGION="us-west-2"
    S3URI="s3://oi-86/k8s"
    CREDDIR="/tmp/kube-aws_output"
  }
  stages {

    stage('kube-aws') {
      steps {
        dir('cluster') {
          sh 'kube-aws render credentials --generate-ca'
          sh 'kube-aws render stack'
          sh 'kube-aws validate --s3-uri $S3URI'
          sh 'kube-aws up --s3-uri $S3URI'
        }
      }
    }

    stage('wait for k8s') {
      steps {
        dir('cluster') {
          retry(60) {
            sleep 10;
            sh 'kubectl --kubeconfig=kubeconfig get nodes';
          }
        }
      }
    }

    stage('Apply k8s manifests') {
      steps {
        sh 'kubectl --kubeconfig=cluster/kubeconfig apply -f k8s'
      }
    }

  }
  post {
    success {
      dir('cluster') {
        echo 'Copying credentials to $CREDDIR'
        sh 'mkdir -p $CREDDIR && cp -a credentials kubeconfig $CREDDIR'
      }
    }
    always {
      deleteDir()
    }
  }
}

