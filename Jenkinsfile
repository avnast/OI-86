pipeline {
  agent any 
  environment {
    AWS_ACCESS_KEY_ID=credentials("aws-key-id")
    AWS_SECRET_ACCESS_KEY=credentials("aws-key")
    EC2_REGION="us-west-2"
    S3URI="s3://oi-86/k8s"
  }
  stages {

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

    stage ('Store credentials for later use') {
      steps {
        dir('cluster') {
          echo 'Copying credentials data to $HOME/userContent'
          sh 'tar czf kubeadmin.tar.gz credentials kubeconfig'
          sh 'mv -f kubeadmin.tar.gz $HOME/userContent'
          echo '$JENKINS_URL/userContent/kubeadmin.tar.gz'
          rtp parserName: 'HTML', stableText: 'Download credentials for accessing k8s from <a href="http://${ENV:JENKINS_URL}/userContent/kubeadmin.tar.gz">http://${ENV:JENKINS_URL}/userContent/kubeadmin.tar.gz</a>'
        }
      }
    }
/*
    stage('Apply k8s manifests (Wordpress)') {
      steps {
        sh 'kubectl --kubeconfig=cluster/kubeconfig apply -f k8s'
      }
    }
*/
  }
  post {
    success {
      deleteDir()
    }
  }
}

