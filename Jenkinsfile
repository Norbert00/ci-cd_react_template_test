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
        stage("Install dependencies") {
            steps {
                script {
                    sh "npm install"
                }
            }
        }
        stage("Tests") {
            steps {
                script {
                    sh "npm test"
                }
            }
        }
        stage("Build package") {
            steps {
                script {
                    sh "npm run build"
                }
            }
        }
        stage("Compile build") {
            steps {
                script {
                    // Define the file path you want to check
                    def filePath = "${env.WORKSPACE}/build"

                    // Use the test command to check if the file exists
                    if (fileExists(filePath)) {
                        echo "File 'build' exists."
                        sh "tar -czvf build.tar.gz ${filePath}"
                        echo "File ${filePath} packed"
                        sh "ls -al"
                        
                        //  step([$class: 'NexusArtifactUploader', nexusVersion: NEXUS_VERSION,
                        //     protocol: NEXUS_PROTOCOL, nexusUrl: NEXUS_URL, version: ARTIFACT_VERSION,
                        //     repository: NEXUS_REPOSITORY, credentialsId: NEXUS_CREDENTIAL_ID,
                        //     artifacts: [
                        //         [artifactId: 'build', classifier: '', file: filePath, type: 'tar.gz']
                        //     ]
                        // ])
                    } else {
                        error "File 'build' does not exist."
                    }
                }
            }
        }
        stage("Upload build to Nexus") {
            steps {
                nexusArtifactUploader artifacts: [
                    [
                        artifactId: 'cra-template-test-app', 
                        classifier: '', 
                        file: '/home/ec2-user/workspace/react-template-app/build.tar.gz', 
                        type: 'tar.gz'
                    ]
                ], 
                        credentialsId: NEXUS_CREDENTIAL_ID, 
                        groupId: 'cra-template-test-app', 
                        nexusUrl: NEXUS_URL, 
                        nexusVersion: NEXUS_VERSION, 
                        protocol: NEXUS_PROTOCOL, 
                        repository: NEXUS_REPOSITORY, 
                        version: '0.1.1'
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
        