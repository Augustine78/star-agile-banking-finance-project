pipeline {
  agent any


  stages {
     stage('checkout'){
       steps {
         echo 'checkout the code from GitRepo'
          git 'https://github.com/Augustine78/star-agile-banking-finance-project.git'
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
               publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, keepAll: false, reportDir: '/var/lib/jenkins/workspace/banking/target/surefire-reports', reportFiles: 'index.html', reportName: 'HTML Report', reportTitles: '', useWrapperFileDirectly: true])    
                    }
            }

     stage('Docker Image Creation'){
               steps {
                      sh 'docker build -t augustine325/bankingapp:latest  .'
                      }
                   }


      stage('Push Image to DockerHub'){
            steps {
                withCredentials([string(credentialsId: 'docker', variable: 'dockervar')]){
                    sh 'docker login -u augustine325 -p ${dockervar}'
                    sh 'docker push augustine325/bankingapp:latest'
	            }
                 }
            }
	
	  stage('Terraform') {
      steps {
        withCredentials([aws(accessKeyVariable: 'AWS_ACCESS_KEY_ID', credentialsId: 'terraform', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY')])  {
          script {
            sh 'terraform init'
            sh 'terraform plan'
            sh 'terraform apply -auto-approve'
          }
        }
      }
    }
	  
   }
}
