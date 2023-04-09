pipeline {
  agent any

  tools {
      maven 'M2_HOME'
      terraform 'terraform-11'
        }
  stages {
     stage('checkout'){
       steps {
         echo 'checkout the code from GitRepo'
          git 'https://github.com/prafullashilimkar/star-agile-banking-finance.git'
                    }
            }
   

     stage('Build the  Application'){
               steps {
                   echo "Cleaning.... Compiling......Testing.........Packaging"
                   sh 'mvn clean package'
                    }
                 }
     stage('publish Reports'){
               steps {
               publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, keepAll: false, reportDir: '/var/lib/jenkins/workspace/Bankingproject/target/surefire-reports', reportFiles: 'index.html', reportName: 'HTML Report', reportTitles: '', useWrapperFileDirectly: true])    
                    }
            }

     stage('Docker Image Creation'){
        steps {
            sh 'docker build -t prafullla/bankingapp:latest  .'
              }
        }
      stage('Push Image to DockerHub'){
               steps {
                   withCredentials([usernamePassword(credentialsId: 'docker-hub', passwordVariable: 'dockerHubPassword', usernameVariable: 'dockerHubUser')]) {
        	   sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPassword}"
                   sh 'docker push prafullla/bankingapp:latest'

	            }
                 }
            }
	     stage('Terraform init'){
        steps {
            sh 'terraform init'
              }

     }    
            stage('Terraform plan'){
        steps {
            sh 'terraform plan'
              }

     }
 stage('Terraform validate'){
        steps {
            sh 'terraform validate'

              }

     }

     stage('Terraform apply'){
        steps {
            sh 'terraform apply -auto-approve'
              } 
	   }
   }
}
