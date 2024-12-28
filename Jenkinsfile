pipeline {
    agent any
    stages {
        stage('SCM') {
            steps {
                git branch: 'main', url: 'https://github.com/Mehar6055/pipeline-demo.git'
            }
        }
        stage('Build') {
            steps {
                sh '''mvn package'''  // The command that actually builds the Maven project
            }
        }
        stage('Archive-Artifacts'){        
            steps{
                archiveArtifacts 'demo_*'
            }
        }
    }
}

