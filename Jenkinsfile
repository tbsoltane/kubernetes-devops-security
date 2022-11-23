pipeline {
  agent any

  stages {
      stage('Build Artifact') {
            steps {
              sh "mvn clean package -DskipTests=true"
              archive 'target/*.jar' //so that they can be downloaded 
            }
        }   


      stage('Unit Tests') {
            steps {
              sh "mvn test"
            }
            post {
              always {
                junit 'target/surefire-reports/*.xml'
                jacoco execPattern: 'target/jacoco.exec'
              }
            }
      }

     stage('Docker Build and Push') {
       steps {
         withDockerRegistry([credentialsId: "docker-hub-tbsoltane-creds", url: ""]) {
           sh 'printenv'
           sh 'sudo docker build -t tbsoltane/numeric-app:""$GIT_COMMIT"" .'
           sh 'docker push tbsoltane/numeric-app:""$GIT_COMMIT""'
         }
       }
     }   
  }
}
