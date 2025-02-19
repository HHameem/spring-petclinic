pipeline {
    agent any

    triggers {
        cron('H/10 * * * 1') 
    }

    environment {
        JAVA_HOME = "C:\\Program Files\\Eclipse Adoptium\\jdk-17"
        PATH = "${JAVA_HOME}\\bin;${env.PATH}"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/HHameem/spring-petclinic.git'
            }
        }

        stage('Build') {
            steps {
                script {
                    bat './mvnw clean package'
                }
            }
        }

        stage('Run Tests and Generate Coverage Report') {
            steps {
                script {
                    bat './mvnw test'
                    bat './mvnw jacoco:report'  
                }
            }
            post {
                always {
                    jacoco execPattern: '**/target/jacoco.exec'
                }
            }
        }

        stage('Archive Artifacts') {
            steps {
                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
            }
        }
    }

    post {
        success {
            echo 'Pipeline executed successfully!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}
