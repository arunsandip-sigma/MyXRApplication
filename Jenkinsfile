pipeline {
    agent any

    environment {
        // 👇 Set your actual SDK path
        ANDROID_HOME = "/var/lib/jenkins/Android/Sdk"
        PATH = "$PATH:$ANDROID_HOME/tools:$ANDROID_HOME/tools/bin:$ANDROID_HOME/platform-tools"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/arunsandip-sigma/MyXRApplication.git',
                    credentialsId: '6375c98a-f69b-4273-ba0c-8c19e9da6274'
            }
        }

        stage('Setup SDK Path') {
            steps {
                // Dynamically create local.properties for Jenkins
                sh '''
                    echo "Dynamically create local.properties for Jenkins"
                    echo "sdk.dir=$ANDROID_HOME" > local.properties
                    echo "✅ Created local.properties with SDK path: $ANDROID_HOME"
                '''
            }
        }

        stage('Build') {
            steps {
                sh 'chmod +x gradlew'
                sh './gradlew clean assembleDebug'
            }
        }

        stage('Archive APK') {
            steps {
                archiveArtifacts artifacts: 'app/build/outputs/apk/debug/*.apk', fingerprint: true
                echo '📦 APK archived successfully!'
            }
        }

        stage('Post Build Notification') {
            steps {
                echo '✅ Build completed successfully!'
                // Later you can add Slack or WhatsApp notifications here
            }
        }
    }
}
