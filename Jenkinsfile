pipeline {
    agent any
    
    environment {
        DIRECTORY_PATH = "/path/to/code/jerkin"
        TESTING_ENVIRONMENT = "school-environment"
        PRODUCTION_ENVIRONMENT = "Kevin"
    }
    
    stages {
        stage('Build') {
            steps {
                echo "Fetching source code from ${DIRECTORY_PATH}"
                echo "Compiling code and generating artifacts"
            }
        }
        
        stage('Test') {
            steps {
                echo "Running unit tests"
                echo "Running integration tests"
            }
        }
        
        stage('Code Quality Check') {
            steps {
                echo "Checking code quality"
            }
        }
        
        stage('Deploy') {
            steps {
                echo "Deploying application to ${TESTING_ENVIRONMENT}"
            }
        }
        
        stage('Approval') {
            steps {
                sleep(time:10,unit:"SECONDS")
            }
        }
        
        stage('Deploy to Production') {
            steps {
                echo "Deploying code to ${PRODUCTION_ENVIRONMENT}"
            }
        }
    }
}