pipeline {
    agent any

    environment {
        MAVEN_OPTS = '-Dmaven.repo.local=$WORKSPACE/.m2/repository'
        // Using workspace local Maven repo to cache dependencies
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'git@github.com:azizmabrouki/atelier-jenkins.git',
                    credentialsId: 'github-ssh'
            }
        }

        stage('Build with Maven') {
            steps {
                // Build once, produce target/*.jar
                sh 'mvn clean package -Dmaven.repo.local=$WORKSPACE/.m2/repository'
            }
            post {
                success {
                    // Archive jar so other stages can use it
                    archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
                }
            }
        }

        stage('SonarQube Analysis') {
            environment {
                scannerHome = tool 'sonar-scanner'
            }
            steps {
                withSonarQubeEnv('sonarqube') {
                    // Reuse compiled classes from Build stage
                    sh """
                        $scannerHome/bin/sonar-scanner \
                        -Dsonar.projectKey=atelier \
                        -Dsonar.sources=src \
                        -Dsonar.java.binaries=target/classes
                    """
                }
            }
        }

        stage('Quality Gate') {
            steps {
                timeout(time: 2, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }

        stage('Docker Build & Run') {
            steps {
                // Reuse jar built in Build stage
                sh 'docker build -t atelier-app:1.0 .'
                sh 'docker run -d -p 8080:8080 atelier-app:1.0'
            }
        }
    }
}

