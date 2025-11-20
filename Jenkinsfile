pipeline {
    agent any

    stages {
        stage('Hello World') {
            steps {
                echo 'Hello World'
            }
        }

        stage('Checkout Code') {
            steps {
                git branch: 'main',
                    url: 'git@github.com:azizmabrouki/atelier-jenkins.git',
                    credentialsId: 'jenkins-ssh-key'
            }
        }

        stage('Build with Maven') {
            steps {
                sh 'mvn clean package'
            }
        }
    }
}
