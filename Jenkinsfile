pipeline {
   environment {
     git_url = "https://github.com/Akki055/java-project.git"
     git_branch = "master"
   }

  agent {label 'dev'}
 
  stages {
    stage('Pull Source') {
      steps {
        git credentialsId: '7a720613-aab2-4b75-a6ca-ddb6ad3764f2', branch: "${git_branch}", url: "${git_url}"
       
      }
     }
    
    stage('Maven Build') {
     steps { 
          sh "mvn clean package && cp target/*.jar . "
     }
    }
     
     stage('Docker Image Build') {     
        steps {
              sh 'sudo docker build -t myjava-image . '
               }
             }
        stage('Docker image push') {
           steps {
                 withCredentials([usernamePassword(credentialsId: '6c94d090-dc88-4929-a8dd-3e95697846f2', passwordVariable: 'Password', usernameVariable: 'Username')]) {
                 sh "sudo docker login -u ${env.Username} -p ${env.Password}"
                 sh "sudo docker image tag myjava-image Akki055/myjava-image:test"
                 sh "sudo docker image push Akki055/myjava-image:test" 
               } 
             }  
          }
      stage('Deploy app') {
         steps {
            sh 'kubectl apply -f app-deploy.yaml'
         }
      }
    }

  post {
    always {
      deleteDir() /* cleanup the workspace */
    }
  }
  }
