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


       stage('SonarQube - SAST') {
       steps {
 //        withSonarQubeEnv('SonarQube') {
           sh "mvn clean verify sonar:sonar -Dsonar.projectKey=test-app -Dsonar.host.url=http://ec2-54-80-83-245.compute-1.amazonaws.com:9000 -Dsonar.login=sqp_b414bc3e7a56eb932398aee7dd597134df482031"
 //        }
 //        timeout(time: 2, unit: 'MINUTES') {
 //          script {
 //            waitForQualityGate abortPipeline: true
 //          }
 //        }
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
