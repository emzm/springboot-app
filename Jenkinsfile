pipeline {
    agent any
//     tools {
//       maven 'maven'
//   }
    stages {

        stage('Build') {
            steps {
               // sh 'mvn -version'
                sh 'mvn clean install'
                sh 'mvn clean package'
               // sh 'mvn clean package -Dmaven.test.failure.ignore=true'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
                //sh 'mvn test -Dmaven.test.failure.ignore=true'
            }
        }
        
        stage('Publishing Junit Tests report ') {
            steps {
                junit 'target/surefire-reports/*.xml'
            }   
        }
        stage('Publishing Code Coverage') {
            steps {
                jacoco()
            }   
        }      
        
        stage('Image push to local Docker registry') {
            steps {
                sh 'docker version'
                sh 'docker build -t dockerregistry.com/springbootjacoco:0.0.1 -f Dockerfile .'
                withDockerRegistry(credentialsId: 'docker-cred', url: 'https://dockerregistry.com/v2/') 
                 {
                 sh 'docker push dockerregistry.com/springbootjacoco:0.0.1'
                 }
             }
         }
    }
}
