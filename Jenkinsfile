pipeline {
    agent any

    stages {
        stage('Clone Repository') {
            steps {
                // Checkout the repository
                git url: 'https://github.com/INTEROYAL/jenkinsproject.git', branch: 'main'
            }
        }

        stage('Build') {
            steps {
                // Install dependencies (if any) and prepare the website for deployment
                // For example, if your website uses Node.js or another build tool
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



        // stage('Test') {
        //     steps {
        //         // Optional: Run automated tests if you have any
        //         // sh 'npm test' or equivalent
        //     }
        // }

        stage('Deployto S3') {
            steps {
                // Deploy your site to a web server (e.g., AWS S3, FTP server, etc.)
                // Example for deploying to an S3 bucket
                withAWS(credentials: 'aws-jenkins-credentials', region: 'us-east-2') {
                    s3Upload(bucket: 'testttting838980', file: 'build/**')
                 }
            }
        }
    }

    post {
        always {
            // Clean up, or send notifications, etc.
            cleanWs()
        }
    }
}
