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
                script {
                    // Define the username and hostname of the production server
                    def remoteUsername = 'your-username'
                    def remoteHostname = 'your-ec2-instance-hostname'
                    
                    // Replace 'your-username' and 'your-ec2-instance-hostname'
                    
                    // Upload application files using SCP
                    sh "scp -i /path/to/private-key.pem -r /path/to/local/application ${remoteUsername}@${remoteHostname}:/path/to/remote/directory"
                    
                    // Execute deployment script remotely
                    sh "ssh -i /path/to/private-key.pem ${remoteUsername}@${remoteHostname} 'cd /path/to/remote/directory && ./deploy.sh'"
                }
            }
        }

    }

    post {
        always {
            mail bcc: '', body: 'Congratulation!!!Successfully transfer email', cc: 'kelvinbui0906115598@gmail.com', from: '', replyTo: '' , subject: 'Congratulation!!!Successfully transfer email', to: 'kelvinbui0906115598@gmail.com'
        }
    }
}
