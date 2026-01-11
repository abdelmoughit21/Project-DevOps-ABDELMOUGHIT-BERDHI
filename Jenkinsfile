pipeline {
    agent any
    
    tools {
        maven 'Maven-3.8'
    }
    
    environment {
        SLACK_CHANNEL = '#jenkins-notifications'
    }
    
    stages {
        stage('Checkout') {
            steps {
                echo '========== Phase 1: Checkout =========='
                checkout scm
                echo 'Code récupéré depuis GitHub'
            }
        }
        
        stage('Build') {
            steps {
                echo '========== Phase 2: Build =========='
                sh 'mvn clean compile'
                echo 'Compilation réussie'
            }
        }
        
        stage('Test') {
            steps {
                echo '========== Phase 3: Test =========='
                sh 'mvn test'
                echo 'Tests exécutés avec succès'
            }
            post {
                always {
                    junit '**/target/surefire-reports/*.xml'
                }
            }
        }
        
        stage('Package') {
            steps {
                echo '========== Phase 4: Package =========='
                sh 'mvn package -DskipTests'
                echo 'Application packagée'
            }
        }
        
        stage('Archive') {
            steps {
                echo '========== Phase 5: Archive =========='
                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
                echo 'Artifacts archivés'
            }
        }
        
        stage('Deploy') {
            steps {
                echo '========== Phase 6: Deploy =========='
                sh 'java -jar target/projet-devops-1.0-SNAPSHOT.jar'
                echo 'Déploiement simulé réussi'
            }
        }
        
        stage('Notify') {
            steps {
                echo '========== Phase 7: Notify =========='
                echo 'Notifications envoyées'
            }
        }
    }
    
    post {
        success {
            echo 'Pipeline exécuté avec succès!'
            slackSend(
                channel: env.SLACK_CHANNEL,
                color: 'good',
                message: "Build SUCCESS: ${env.JOB_NAME} #${env.BUILD_NUMBER}\nLien: ${env.BUILD_URL}"
            )
        }
        failure {
            echo 'Le pipeline a échoué.'
            slackSend(
                channel: env.SLACK_CHANNEL,
                color: 'danger',
                message: "Build FAILED: ${env.JOB_NAME} #${env.BUILD_NUMBER}\nLien: ${env.BUILD_URL}"
            )
        }
    }
}