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
        stage ("Install dependencies") {
            steps {
                script {
                    sh "npm install"
                }
            }
        }
        stage ("Test stage") {
            steps {
                script {
                    sh "npm test"
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
        stage ("Pack build") {
            steps {
                script {
                    // Define the file path you want to check
                    def filePath = "${env.WORKSPACE}/build"
                    // Use the test command to check if the file exists
                    if (fileExists(filePath)) {
                        echo "File 'build' exists."
                        sh "tar -czvf build.tar.gz ${filePath}/build"

                } else {
                        error "File 'build' does not exist."
                }
            }
        }
    }
    post {
        always {
            cleanWs()
        }
    }
}

        // stage ("Upload package to nexus") {
        //     steps {
        //         script {
        //             nexusArtifactUploader(
        //                     nexusVersion: NEXUS_VERSION,
        //                     protocol: NEXUS_PROTOCOL,
        //                     nexusUrl: NEXUS_URL,
        //                     version: ARTIFACT_VERSION,
        //                     repository: NEXUS_REPOSITORY,
        //                     credentialsId: NEXUS_CREDENTIAL_ID,
        //             )
        //         }
        //     }
        // }
        