@Library("shared-library") _
pipeline {
    environment {
        ARTIFACTORY_ID  = "fb20cbba-98e0-4fe9-b32a-34f38885f66f"
        ARTIFACTORY_URL = "https://artifactory.adesso-insure.de/artifactory"
        EAR_ARTEFACT_ID = "insuria-paytras-ear"
		EAR_CLASSIFIER  = "install"
		EAR_GROUP_ID    = "insuria.paytras"
		EAR_REPOSITORY  = "ipay-maven"
    }
    agent { label "linux" }
    tools { maven "maven-3" }
    options { buildDiscarder(logRotator(numToKeepStr: '10')); }
    parameters {
        string(name: 'VERSION_PAYTRAS', defaultValue: '2023.1.0.RC5')
    }
    stages {
        stage('Set variables') {
            steps {
                script {
                    sh "echo Artifactory ID is ${ARTIFACTORY_ID}"
                    sh "echo Artifactory URL is ${ARTIFACTORY_URL}"
                    sh "echo Artifact repository is ${EAR_REPOSITORY}"
                    sh "echo Artifact ID is ${EAR_ARTEFACT_ID}"
                    sh "echo Artifact group ID is ${EAR_GROUP_ID}"
                    sh "echo Artifact classifier is ${EAR_CLASSIFIER}"
                }
            }
        }
        stage('Copy artifacts') {
            steps {
                withCredentials([usernamePassword(credentialsId: ARTIFACTORY_ID, passwordVariable: 'ARTIFACTORY_CREDENTIALS_PSW', usernameVariable: 'ARTIFACTORY_CREDENTIALS_USR')]) {
                    dir('./tmp') {
                        script {
                            fromJFrog.getArtifact(ARTIFACTORY_URL,EAR_REPOSITORY,EAR_GROUP_ID,EAR_ARTEFACT_ID,VERSION_PAYTRAS,EAR_CLASSIFIER)
                            sh "ls -la"
                        }   
                    }
                }
            }
        }
        
    }
}