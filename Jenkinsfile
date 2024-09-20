pipeline {
    agent any

    tools {
        nodejs 'NodeJS16' // Use the name configured in Global Tool Configuration
    }

    stages {
        stage('Clone Repository') {
            steps {
                // Checkout the repository
                git url: 'https://github.com/INTEROYAL/jenkinsproject.git', branch: 'main'
            }
        }

        stage('Check Node.js and npm Versions') {
            steps {
                // Check Node.js and npm versions
                sh 'node --version'
                sh 'npm --version'
            }
        }

        stage('Build') {
            steps {
                // Install dependencies and prepare the website for deployment
                sh 'npm install' // Change to the command suitable for your tech stack
                sh 'npm run build' // Adjust as necessary for your website's build process
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

        // Optional Test Stage
        // stage('Test') {
        //     steps {
        //         // Optional: Run automated tests if you have any
        //         // sh 'npm test' or equivalent
        //     }
        // }

        stage('Deploy to S3') {
            steps {
                // Deploy your site to an S3 bucket
                withAWS(credentials: 'aws-jenkins-credentials', region: 'us-east-2') {
                    s3Upload(bucket: 'testttting83898', file: 'build/**')
                }
            }
        }
    }

    post {
        always {
            // Clean up workspace
            cleanWs()
        }
    }
}
