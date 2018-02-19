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
    stage('kube-aws init') {
      steps {
        dir('cluster') {
          sh 'kube-aws init --cluster-name=avnast-k8s-cluster --external-dns-name=avnast_k8s.inkubator.opsworks.io --hosted-zone-id=Z3DT1QHQCXWAFD --region=us-west-2 --key-name=av.nast --availability-zone=us-west-2b --kms-key-arn="arn:aws:kms:us-west-2:762510625864:key/3ad2bd66-9fbc-4e6d-9ea8-c6b4c1cc6617"'
        }
      }
    }

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
        sh 'zip cluster.zip cluster'
        sh 'ls -la'
        archiveArtifacts(artifacts: 'cluster.zip', fingerprint: true)
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

