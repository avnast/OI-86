pipeline {
  agent any 
  environment {
    AWS_ACCESS_KEY_ID=credentials("aws-key-id")
    AWS_SECRET_ACCESS_KEY=credentials("aws-key")
    EC2_REGION="us-west-2"
    S3URI="s3://oi-86/k8s"
  }
  stages {
/*
    stage('kube-aws render') {
      steps {
        dir('cluster') {
          sh 'kube-aws render credentials --generate-ca'
          sh 'kube-aws render stack'
        }
      }
    }

    stage('kube-aws validate') {
      steps {
        dir('cluster') {
          sh 'kube-aws validate --s3-uri $S3URI'
        }
      }
    }

    stage('kube-aws up') {
      steps {
        dir('cluster') {
          sh 'kube-aws up --s3-uri $S3URI'
        }
      }
    }

    stage('wait for k8s') {
      steps {
        dir('cluster') {
          retry(60) {
            sleep 10
            sh 'kubectl --kubeconfig=kubeconfig get nodes'
          }
        }
      }
    }
*/
    stage ('Store credentials for later use') {
      steps {
        sh 'printenv'
        echo 'Copying data to $HOME/userContent'
        sh 'tar czf cluster.tar.gz cluster'
        sh 'mv -f cluster.tar.gz $HOME/userContent'
        input message: 'Download http://$JENKINS_URL/userContent/cluster.tar.gz and click "Proceed" (then it will be DELETED)'
        sh 'rm -f $HOME/userContent/cluster.tar.gz'
      }
    }

    stage('Apply k8s manifests (Wordpress)') {
      steps {
        sh 'kubectl --kubeconfig=cluster/kubeconfig apply -f k8s'
      }
    }

  }
  post {

    always {
      deleteDir()
    }

  }
}

