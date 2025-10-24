pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/arunsandip-sigma/MyXRApplication.git',
                    credentialsId: '6375c98a-f69b-4273-ba0c-8c19e9da6274'
            }
        }

        stage('Build') {
            steps {
                sh 'chmod +x gradlew'
                sh './gradlew clean assembleDebug'
            }
        }

        stage('Post Build Notification') {
            steps {
                echo '✅ Build completed successfully!'
                // Later: add WhatsApp message script here
            }
        }
    }
}
