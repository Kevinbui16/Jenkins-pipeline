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
                // Use SonarQube to analyze the code
                withSonarQubeEnv('SonarQube') {
                    sh 'mvn sonar:sonar'
                }
            }
        }
        stage('Security Scan') {
            steps {
                // Run security scans using tools like OWASP Dependency-Check
                sh 'dependency-check.sh'
            }
        }
        stage('Deploy to Staging') {
            steps {
                // Deploy to staging server (e.g., AWS EC2)
                sh 'jenkins-deploy-staging.sh'
            }
        }
        stage('Integration Tests on Staging') {
            steps {
                // Run integration tests on staging
                sh 'selenium-tests.sh'
            }
        }
        stage('Deploy to Production') {
            steps {
                // Deploy to production server (e.g., AWS EC2)
                sh 'jenkins-deploy-production.sh'
            }
        }
    }

    post {
        failure {
            // Send email on pipeline failure
            emailext(
                subject: "Pipeline Failed: ${currentBuild.fullDisplayName}",
                body: "The pipeline ${currentBuild.fullDisplayName} has failed.",
                attachLog: true,
                recipientProviders: [[$class: 'CulpritsRecipientProvider']]
            )
        }
        success {
            emailext(
                subject: "Pipeline Succeeded: ${currentBuild.fullDisplayName}",
                body: "The pipeline ${currentBuild.fullDisplayName} has succeeded.",
                attachLog: true,
                to: 'kelvin0906115598@gmail.com', // Add the recipient email here
                recipientProviders: [[$class: 'CulpritsRecipientProvider']]
            )
        }
    }
}
