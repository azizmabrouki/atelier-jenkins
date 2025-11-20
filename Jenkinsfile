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
                    credentialsId: 'SHA256:PGY5wo8q5xaUnZcsqEzkuzs59OL3BBU5kEmsmqo2f3c'
            }
        }

        stage('Build with Maven') {
            steps {
                sh 'mvn clean package'
            }
        }
    }
}
