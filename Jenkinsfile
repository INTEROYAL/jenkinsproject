pipeline {
    agent any

    stages {
        stage('Clone Repository') {
            steps {
                git url: 'https://github.com/INTEROYAL/jenkinsproject.git', branch: 'main', credentialsId: 'github-pat'
            }
        }

        stage('Create S3 Bucket') {
            steps {
                script {
                    def bucketName = 'testttting83898'
                    def bucketExists = sh(script: "aws s3api head-bucket --bucket ${bucketName} --output text 2>&1 || echo 'No'", returnStdout: true).trim()
                    if (bucketExists == 'No') {
                        sh "aws s3api create-bucket --bucket ${bucketName} --region us-east-1"
                    }
                }
            }
        }

        stage('List Files') {
            steps {
                sh 'ls -al'
            }
        }

        stage('Deploy to S3') {
            steps {
                withAWS(credentials: 'aws-jenkins-credentials', region: 'us-east-2') {
                    s3Upload(bucket: 'testttting83898', file: '*') // Adjusted file path
                }
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}
