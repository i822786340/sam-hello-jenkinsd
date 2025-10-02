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
                sh 'mvn test'
            }
        }

        stage('Deploy with SAM') {
            when {
                branch 'main'
            }
            steps {
                sh '''
                    sam deploy \
                        --stack-name sam-hello-jenkinsd \
                        --capabilities CAPABILITY_IAM \
                        --no-confirm-changeset \
                        --resolve-s3
                '''
            }
        }
    }

    post {
        always {
            junit '**/target/surefire-reports/*.xml'
        }
    }
}
