pipeline {
    agent any
    tools {
        maven 'Maven 3'
    }
    environment {
        SONAR_TOKEN = credentials('SonarQubeTokenID') // Replace with your credential ID
    }
    stages {
        stage('SCM') {
            steps {
                git branch: 'main', url: 'https://github.com/Mehar6055/pipeline-demo.git'
            }
        }
        stage('Build') {
            steps {
                sh 'mvn package'
            }
        }
        stage('Archive-Artifacts') {
            steps {
                archiveArtifacts 'target/demo_*'
            }
        }
        stage('Sonar-Stage') {
            steps {
                withSonarQubeEnv('SonarQube') { // Ensure this matches your Jenkins configuration
                    sh 'mvn clean package sonar:sonar -Dsonar.login=$SONAR_TOKEN'
                }
            }
        }
        stage('Quality Gate') {
            steps {
                sleep(60)
                script {
                    def qg = waitForQualityGate()
                    if (qg.status != 'OK') {
                        error "Quality Gate failed: ${qg.status}"
                    }
                }
            }
        }
        stage('Upload-Artifacts') {
            steps {
                nexusArtifactUploader artifacts: [[artifactId: 'dev', classifier: '', file: "./target/demo_artifact-1.1.jar", type: 'jar']], 
                credentialsId: 'nexus', groupId: 'dev_group', nexusUrl: '13.61.10.180:8081/nexus', 
                nexusVersion: 'nexus2', protocol: 'http', repository: 'RepoR', version: '1.1'
            }
        }
    }
}
