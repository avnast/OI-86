pipeline {
    agent any 
    stages {
        stage('Stage 1') {
            steps {
                sh 'which kubectl'
            }
        }
        stage('Check kubectl') {
            steps {
              withKubeConfig(caCertificate: '''-----BEGIN CERTIFICATE-----
MIIC6DCCAdCgAwIBAgIBADANBgkqhkiG9w0BAQsFADAlMREwDwYDVQQKEwhrdWJl
LWF3czEQMA4GA1UEAxMHa3ViZS1jYTAeFw0xODAyMTYxMTU5MzlaFw0yODAyMTQx
MTU5MzlaMCUxETAPBgNVBAoTCGt1YmUtYXdzMRAwDgYDVQQDEwdrdWJlLWNhMIIB
IjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEA91NAL0O6WUeif5lbwl1bJP1H
cLYjTQobmigckFP9/dZpOcLqiQ1GCwRG9565YexaQ/t1QrPzGrwXgiKk+hGlZuH+
7ESpEJTlOyWND8Vv4LEP0j0D4DhsPcFs6YfsFvrK3TzOLHB1pP6ZGgmkR2lT+83A
bBVZXqlFK4WqFXJ1trjDSP9XJ0TtKw37HbPF1dbwqW7hHVJ3TOsNCpOdLYYn7/Zg
x3FnjTM6bcujwSGoVfV1Sh/EJesi+l6ouiJ9yDzoY5/FJVRpxA4XyYO0wFlHLl7h
K8p+9mDDNmaPwT5H6NmPIOawgIvFZyoFyob8t7DFDj54rfhTaQ+gWkqvH7ktkwID
AQABoyMwITAOBgNVHQ8BAf8EBAMCAqQwDwYDVR0TAQH/BAUwAwEB/zANBgkqhkiG
9w0BAQsFAAOCAQEA84FfOPNo8AdUrCCzEdbrkFSxjm67IK4I+cYkzDCWjSXBqU3B
DYIAE8Evs4g6dwZpm9HrZbssINzhzIZc9r9xYZnRihfo2O8I/ZsJ0uFN6v7drn6p
l1MMqP5c2VMDHguBvQZUDuI6GiF0TxNJA7/hCcvq4a0AQtxElLOMCDqlbh0I7UsQ
m/2X4nApVhsv3b1191AIaTUDLqP35aR4b83z9RaiUGLLlwPzsxNj9razTVcyQaY9
87poWoMpnuAME0U5zTplXrozY4InTV7z6mJGS8sENHHVkSqrR1oJEPSWk6n2jsOZ
3E8AQVyNa7+nQ8G0Ff8NW2PrFsbcFUJraKpo5g==
-----END CERTIFICATE-----
''', credentialsId: '9369e911-dc8e-45ce-961e-c18c6e517c3f', serverUrl: 'https://avnast_k8s.inkubator.opsworks.io') {
                    sh 'kubectl get nodes'
                }
            }
        }
    }
}

