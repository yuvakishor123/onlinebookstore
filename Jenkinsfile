pipeline {
    agent any

    tools {
        maven 'Maven3'   // Make sure Maven is configured in Jenkins -> Manage Jenkins -> Tools
        jdk 'JDK17'      // Optional, if using Java 11 project
    }

    environment {
        // Nexus repository details
        NEXUS_VERSION = 'nexus3'
        NEXUS_PROTOCOL = 'http'
        NEXUS_URL = 'http://13.201.226.254:8081'
        NEXUS_REPOSITORY = 'maven-releases'         // Or maven-snapshots if using snapshot versions
        NEXUS_CREDENTIAL_ID = 'nexus-cred'          // Jenkins credentials ID for Nexus login
        GROUP_ID = 'com.example'
        ARTIFACT_ID = 'myapp'
        VERSION = '1.0.0'
        PACKAGING = 'jar'
    }

    stages {

        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/yuvakishor123/onlinebookstore.git'
            }
        }

        stage('Build with Maven') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Upload Artifact to Nexus') {
            steps {
                script {
                    def artifactPath = "target/${ARTIFACT_ID}-${VERSION}.${PACKAGING}"

                    nexusArtifactUploader(
                        nexusVersion: "${NEXUS_VERSION}",
                        protocol: "${NEXUS_PROTOCOL}",
                        nexusUrl: "${NEXUS_URL}",
                        groupId: "${GROUP_ID}",
                        version: "${VERSION}",
                        repository: "${NEXUS_REPOSITORY}",
                        credentialsId: "${NEXUS_CREDENTIAL_ID}",
                        artifacts: [
                            [artifactId: "${ARTIFACT_ID}", classifier: '', file: artifactPath, type: "${PACKAGING}"]
                        ]
                    )
                }
            }
        }
    }

    post {
        success {
            echo "✅ Build and artifact upload successful! Artifact stored in Nexus."
        }
        failure {
            echo "❌ Build or Nexus upload failed. Please check logs."
        }
    }
}
