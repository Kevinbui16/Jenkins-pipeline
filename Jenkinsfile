pipeline {
    agent any
    tools {
        // Use the configured Maven installation
        maven 'Maven'
        jdk 'jdk8'
        
    }
    stages {
        stage('Checkout') {
            steps {
                // Checkout the repository
                checkout scm
            }
        }
        stage('Build') {
            steps {
                // Navigate to the directory containing the pom.xml
                    
                // Build using Maven
                sh 'mvn clean package'
                
            }
        }
        stage('Unit and Integration Tests') {
            steps {
                // Run unit tests
                sh 'mvn test'
                // Run integration tests
                sh 'mvn integration-test'
            }
        }
        stage('Code Analysis') {
            steps {
                // Use an online code analysis service such as Codacy, Code Climate, or SonarCloud
                sh 'curl -X POST -F "projectToken=YOUR_PROJECT_TOKEN" -F "commitUUID=${GIT_COMMIT}" https://api.codacy.com/2.0/coverage/${BUILD_NUMBER}/java'
            }
        }
        stage('Security Scan') {
            steps {
                // Use an online security scanning service such as Snyk, WhiteSource Bolt, or Black Duck
                sh 'curl -X POST -H "Content-Type: application/json" -H "Authorization: token YOUR_API_TOKEN" -d @snyk.json https://snyk.io/api/v1/test'
            }
        }
        
       stage('Deploy to Staging') {
            steps {
                // Configure Git user for this session
                sh 'git config user.email "you@example.com"'
                sh 'git config user.name "Your Name"'
        
                // Commit and push changes to the GitLab repository
                sh 'git add .'
                sh 'git commit -m "Deploy to staging"'
                sh 'git push origin HEAD:main'
            }
        }
        stage('Integration Tests on Staging') {
            steps {
                // Use JUnit and Selenium to run integration tests on the staging environment
                sh 'mvn test'
            }
        }
        stage('Deploy to Production') {
            steps {
                // Package the application into a ZIP file
                sh 'zip -r my-application.zip .'
                // Upload the ZIP file to an S3 bucket
                withCredentials([string(credentialsId: 'my-aws-credentials', variable: 'AWS_CREDENTIALS')]) {
                    sh 'aws s3 cp my-application.zip s3://my-s3-bucket/my-application.zip'
                }
                // Deploy the application to Elastic Beanstalk
                sh 'aws elasticbeanstalk create-application-version --application-name my-application --version-label v1 --source-bundle S3Bucket=my-s3-bucket,S3Key=my-application.zip'
                sh 'aws elasticbeanstalk update-environment --environment-name my-production-environment --version-label v1'
            }
        }
    }

    post {
        always {
            mail bcc: '', body: 'Thong bao ket qua build', cc: 'kelvinbui0906115598@gmail.com', from: '', replyTo: '' , subject: 'Thong bao ket qua build', to: 'kelvinbui0906115598@gmail.com'
        }
    }
}
