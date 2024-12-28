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
<<<<<<< HEAD
                sh '''mvn package'''
=======
                sh 'echo "This step will build the mvn"'
>>>>>>> 195ddbf484b1f68dfe76fa5e88ba56deedb86e77
            }
        }
    }
}
