pipeline {
    agent {
        docker {
            image "node:slim"
        }
    }
    environment {
                // This can be nexus3 or nexus2
        NEXUS_VERSION = "nexus3"
        // This can be http or https
        NEXUS_PROTOCOL = "https"
        // Where your Nexus is running
        NEXUS_URL = "nexus.n00dns.co"
        // Repository where we will upload the artifact
        NEXUS_REPOSITORY = "react_app_npm_group_repo"
        // Jenkins credential id to authenticate to Nexus OSS
        NEXUS_CREDENTIAL_ID = "jenkins_nexus"
        ARTIFACT_VERSION = "${BUILD_NUMBER}"
    }

    stages {

        stage ("Test stage") {
            steps {
                script {
                    sh "cd app && npm test"
                }
            }
        }

        stage ("Build package") {
            steps {
                script {
                    sh "npm run build"
                }
            }
        }

        stage ("Upload package to nexus") {
            steps {
                script {
                    nexusArtifactUploader(
                            nexusVersion: NEXUS_VERSION,
                            protocol: NEXUS_PROTOCOL,
                            nexusUrl: NEXUS_URL,
                            version: ARTIFACT_VERSION,
                            repository: NEXUS_REPOSITORY,
                            credentialsId: NEXUS_CREDENTIAL_ID,
                    )
                }
            }
        }
    }
}