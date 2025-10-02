pipeline {
    agent any

    environment {
        AWS_DEFAULT_REGION = "us-east-1"   // change if needed
        SAM_TEMPLATE = "template.yaml"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build with SAM') {
            steps {
                sh 'sam build'
            }
        }

        stage('Run Unit Tests') {
            steps {
                dir('HelloWorldFunction') {
                    sh 'mvn test'
                }
            }
        }

        stage('Deploy with SAM') {
            steps {
                withCredentials([[
                    $class: 'AmazonWebServicesCredentialsBinding',
                    credentialsId: '52929e35-4f2f-4826-8b91-d1830ed6744d'
                ]]) {
                    sh 'sam deploy --stack-name sam-hello-jenkinsd --capabilities CAPABILITY_IAM --no-confirm-changeset --resolve-s3'
                }
            }
        }
    }

    post {
        always {
            junit '**/target/surefire-reports/*.xml'
        }
    }
}
