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
                    qg=waitForQualityGate('sonarQuality')
                    if (qg.status == 'OK'){
                        echo "Quality Gate passed"
                    }
                    else {
                        error "The pipeline failed due to my quality gate check"
                    }
                }
            }
        }
        stage('Upload-Artifacts') {
            steps {
                nexusArtifactUploader artifacts: [[artifactId: 'jenkins', classifier: '', file: "./target/demo_artifact-1.0-SNAPSHOT.jar", type: 'jar']], 
                credentialsId: 'nexus_cred', groupId: 'jenkins', nexusUrl: '16.170.253.109:8081/repository/demorepo/', 
                nexusVersion: 'NEXUS3', protocol: 'http', repository: 'demorepo', version: '1.1'
            }
        }
    }
}
