pipeline {
    agent {
        docker {
            image "node:slim"
        }
    }
    environment {
        //This can be nexus3 or nexus2
        NEXUS_VERSION = "nexus3"
        // This can be http or https
        NEXUS_PROTOCOL = "https"
        // Where your Nexus is running
        NEXUS_URL = "nexus.n00dns.co"
        // Repository where we will upload the artifact
        NEXUS_REPOSITORY = "react-app"
        // Jenkins credential id to authenticate to Nexus OSS
        NEXUS_CREDENTIAL_ID = "jenkins_nexus"
        // Version of package
        PACKAGE_JSON_VERSION = ''
    }
    stages {
        stage('Read JSON') {
         steps {
            script {
                def packageJSON = readJSON file: 'package.json'
                PACKAGE_JSON_VERSION = packageJSON.version
                echo "Package JSON Version: ${PACKAGE_JSON_VERSION}"
            }
         }
      }
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
                        sh "cd ${filePath}; tar -czvf build.tgz *"
                        echo "File ${filePath} packed"
                        sh "ls -al"
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
                        artifactId: 'react-app', 
                        classifier: '', 
                        file: "${env.WORKSPACE}/build.tgz", 
                        type: 'tgz'
                    ]
                ], 
                        credentialsId: NEXUS_CREDENTIAL_ID, 
                        groupId: 'jenkins', 
                        nexusUrl: NEXUS_URL, 
                        nexusVersion: NEXUS_VERSION, 
                        protocol: NEXUS_PROTOCOL, 
                        repository: NEXUS_REPOSITORY, 
                        version: "${PACKAGE_JSON_VERSION}"
            }
        }
        stage("Triger cd_s3_bucket_upload job") {
            steps {
                build job: 'cd_s3_bucket_upload', parameters: [[$class: 'StringParameterValue', name: 'VERSION', value: PACKAGE_JSON_VERSION]]
            }
        }
    }
    post {
        always {
            cleanWs()
        }
    }
}