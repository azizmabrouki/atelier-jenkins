pipeline {
    agent any

    environment {
        MAVEN_HOME = "/usr/share/maven"  // adjust if needed
    }

    stages {

        stage('Hello World') {
            steps {
                echo 'Hello World'
            }
        }

        stage('Checkout Code') {
            steps {
                // Checkout from GitHub using SSH and Jenkins credentials
                git branch: 'main',
                    url: 'git@github.com:azizmabrouki/atelier-jenkins.git',
                    credentialsId: 'jenkins-ssh-key'
            }
        }

        stage('Build with Maven') {
            steps {
                // Ensure Maven is installed on the Jenkins agent/container
                sh 'mvn clean package'
            }
        }

        stage('Post-build Info') {
            steps {
                echo 'Pipeline finished successfully.'
                sh 'ls -l target' // optional: list build artifacts
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed without errors!'
        }
        failure {
            echo 'Pipeline failed. Check the console output for errors.'
        }
    }
}
