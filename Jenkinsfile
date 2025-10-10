pipeline {
    agent any

    tools {
        maven 'maven3'    // Make sure this Maven is configured in Jenkins -> Manage Jenkins -> Tools
        jdk 'JDK7'
    }

    stages {

        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/yuvakishor123/onlinebookstore.git'
            }
        }

        stage('Build Project') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Upload Artifact to Nexus') {
            steps {
                script {
                    nexusArtifactUploader(
                        nexusVersion: 'nexus3',
                        protocol: 'http',
                        nexusUrl: 'http://13.201.226.254:8081',
                        groupId: 'com.example',
                        version: '1.0.0',
                        repository: 'maven-releases',
                        credentialsId: 'nexus-cred',
                        artifacts: [
                            [artifactId: 'myapp', classifier: '', file: 'target/myapp-1.0.0.jar', type: 'jar']
                        ]
                    )
                }
            }
        }
    }

    post {
        success {
            echo '✅ Artifact successfully uploaded to Nexus repository!'
        }
        failure {
            echo '❌ Build or upload failed. Please check the logs.'
        }
    }
}
