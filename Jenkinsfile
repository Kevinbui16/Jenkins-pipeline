pipeline {
    agent any
    tools {
        // Use the configured Maven installation
        maven 'maven2'
    }
    stages {
        stage('Build') {
            steps {
                // Build the code using a build automation tool such as Maven
                sh 'mvn clean package'
            }
        }
        stage('Unit and Integration Tests') {
            steps {
                // Run unit and integration tests using test automation tools such as JUnit or TestNG
                sh 'mvn test'
            }
        }
        stage('Code Analysis') {
            steps {
                // Integrate a code analysis tool such as SonarQube to analyze the code and ensure it meets industry standards
                withSonarQubeEnv('SonarQube') {
                    sh 'mvn sonar:sonar'
                }
            }
        }
        stage('Security Scan') {
            steps {
                // Perform a security scan on the code using a tool such as OWASP ZAP to identify any vulnerabilities
                sh 'zap-cli quick-scan --self-contained --start-options "-config api.disablekey=true" http://localhost:8080'
            }
        }
        stage('Deploy to Staging') {
            steps {
                // Deploy the application to a staging server such as an AWS EC2 instance
                sh 'scp target/my-app.war ec2-user@staging-server:/var/lib/tomcat8/webapps/'
            }
        }
        stage('Integration Tests on Staging') {
            steps {
                // Run integration tests on the staging environment to ensure the application functions as expected in a production-like environment
                sh 'mvn verify -Dskip.surefire.tests=true -Dskip.unit.tests=true'
            }
        }
        stage('Deploy to Production') {
            steps {
                // Deploy the application to a production server such as an AWS EC2 instance
                sh 'scp target/my-app.war ec2-user@production-server:/var/lib/tomcat8/webapps/'
            }
        }
    }
}
