pipeline{
agent any
   stages{
      stage(’SCM’){
          steps{
                  git 'https://github.com/Mehar6055/pipeline-demo.git'
               }
           }
          stage('Build'){
          steps{
           sh ' ' ' mvn package ' ' '
                       
          }
        }
      }
   }
