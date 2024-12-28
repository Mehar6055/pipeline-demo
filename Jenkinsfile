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
    }
}

