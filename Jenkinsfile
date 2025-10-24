pipeline {
    agent any

    environment {
        // Android SDK path
        ANDROID_HOME = "/var/lib/jenkins/Android/Sdk"
        PATH = "$PATH:$ANDROID_HOME/tools:$ANDROID_HOME/tools/bin:$ANDROID_HOME/platform-tools"

        // WhatsApp notification settings (CallMeBot)
        CALLMEBOT_PHONE = '+919438551357'
        CALLMEBOT_API_KEY = credentials('callmebot-api-key')  // 👈 Add this to Jenkins credentials
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
    }

    post {
        success {
            script {
                def message = "✅ BUILD SUCCESS\nJob: ${env.JOB_NAME}\nBuild: #${env.BUILD_NUMBER}\nDuration: ${currentBuild.durationString}\n${env.BUILD_URL}"
                sendWhatsApp(message)
                echo '✅ Build completed successfully!'
            }
        }

        failure {
            script {
                def message = "❌ BUILD FAILED\nJob: ${env.JOB_NAME}\nBuild: #${env.BUILD_NUMBER}\n${env.BUILD_URL}"
                sendWhatsApp(message)
                echo '❌ Build failed!'
            }
        }

        unstable {
            script {
                def message = "⚠️ BUILD UNSTABLE\nJob: ${env.JOB_NAME}\nBuild: #${env.BUILD_NUMBER}\n${env.BUILD_URL}"
                sendWhatsApp(message)
            }
        }
    }
}

def sendWhatsApp(String message) {
    try {
        def encodedMessage = java.net.URLEncoder.encode(message, "UTF-8")
        sh """
            curl -s "https://api.callmebot.com/whatsapp.php?phone=${env.CALLMEBOT_PHONE}&text=${encodedMessage}&apikey=${env.CALLMEBOT_API_KEY}"
        """
        echo "📱 WhatsApp notification sent!"
    } catch (Exception e) {
        echo "⚠️ Failed to send WhatsApp notification: ${e.message}"
    }
}