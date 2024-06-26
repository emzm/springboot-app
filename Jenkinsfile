pipeline {
    agent any
//     tools {
//       maven 'maven'
//   }
    stages {
        stage('Checkout') {
            steps {
                script {
                   //checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: 'github-pat-', url: 'https://github.com/rehmanjaffar/nodeApp.git']])
                //   checkout([$class: 'GitSCM',
                //              branches: [[name: '*/master']],
                //              extensions: [[$class: 'SparseCheckoutPaths', paths: ['reactApp-master']]],
                //              userRemoteConfigs: [[credentialsId: 'github-pat',
                //              url: 'https://github.com/UsmanAsim11/projects.git']]
                //     ])
                checkout([$class: 'GitSCM',
                            branches: [[name: '*/main']],
                            doGenerateSubmoduleConfigurations: false,
                            extensions: [
                            [$class: 'SparseCheckoutPaths', sparseCheckoutPaths:[[$class:'SparseCheckoutPath', path:'springboot-app/'],[$class:'SparseCheckoutPath', path:'nodeApplication/']]],
                            ],
                            submoduleCfg: [],
                            userRemoteConfigs: [[credentialsId: 'github-pat',
                            url: 'https://github.com/emzm/jenkins_pipelines_all_apps.git']]])
                }
            }
        }
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
