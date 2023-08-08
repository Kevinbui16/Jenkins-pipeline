pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
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
                // Run code analysis using tools like SonarQube
                sh 'sonar-scanner'
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
            // Send email on pipeline success
            emailext(
                subject: "Pipeline Succeeded: ${currentBuild.fullDisplayName}",
                body: "The pipeline ${currentBuild.fullDisplayName} has succeeded.",
                attachLog: true,
                recipientProviders: [[$class: 'CulpritsRecipientProvider']]
            )
        }
    }
}
