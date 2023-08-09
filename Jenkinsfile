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
                    // Define the IP address of your deployment server
                    def remoteIP = 'xxx.xxx.xxx.xxx'  // Replace with the actual IP address
                    
                    // Prepare the deployment payload (if needed)
                    def deploymentPayload = '{"key": "value"}'
                    
                    // Trigger remote deployment using curl with IP address
                    sh "curl -X POST -H 'Content-Type: application/json' -d '${deploymentPayload}' http://${remoteIP}/deploy"
                }
            }
        }
    }

    post {
        always {
            mail bcc: '', body: 'Thong bao ket qua build', cc: 'kelvinbui0906115598@gmail.com', from: '', replyTo: '' , subject: 'Thong bao ket qua build', to: 'kelvinbui0906115598@gmail.com'
        }
    }
}
