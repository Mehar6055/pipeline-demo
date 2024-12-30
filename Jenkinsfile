pipeline {
    agent any
    tools {
        maven 'Maven 3'  // This must match the name configured in Jenkins' Global Tool Configuration
    }
    stages {
        stage('SCM') {
            steps {
                git branch: 'main', url: 'https://github.com/Mehar6055/pipeline-demo.git'
            }
        }
        stage('Build') {
            steps {
                sh 'mvn package'  // This will now work because Maven is installed and configured
            }
        }
        stage('Archive-Artifacts') {
            steps {
                archiveArtifacts 'target/demo_*'
            }
        }
        stage('Sonar-Stage') {
            steps {
                withSonarQubeEnv('SonarQube') { // Ensure 'SonarQube' matches the name configured in Jenkins
                    sh 'mvn clean package sonar:sonar -Dsonar.projectKey=pipeline-demo -Dsonar.host.url=http://<SONARQUBE_SERVER>:9000 -Dsonar.login=<SONAR_TOKEN>'
                }
            }
        }
        stage('Quality Gate') {
            steps {
                script {
                    timeout(time: 2, unit: 'MINUTES') {  // Wait for a maximum of 2 minutes for the quality gate
                        def qg = waitForQualityGate()
                        if (qg.status == 'OK') {
                            echo "Quality gate passed"
                        } else {
                            error "The pipeline failed due to quality gate check: ${qg.status}"
                        }
                    }
                }
            }
        }
        stage('Upload-Artifacts') {
            steps {
                nexusArtifactUploader(
                    artifacts: [[artifactId: 'demo_artifact', classifier: '', file: './target/demo_artifact-1.1.jar', type: 'jar']],
                    credentialsId: 'nexus', 
                    groupId: 'dev_group', 
                    nexusUrl: '16.171.147.53:8081/nexus', 
                    nexusVersion: 'nexus2', 
                    protocol: 'http', 
                    repository: 'RepoR', 
                    version: '1.1'
                )
            }
        }
    }
    post {
        always {
            echo 'Pipeline execution finished. Cleaning workspace...'
            cleanWs()
        }
    }
}
