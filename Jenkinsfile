pipeline {
    agent any
    tools {
        maven 'maven'
    }
    stages {
        stage('Checkout From Git') {
            steps {
                git branch:'prod' , url: "https://github.com/sudhakar527/aws-jenkins-springboot.git"
            }
        }
        stage ('Maven Parallel Stages') {
         parallel {
        //  stage ('Maven Validate'){
        //     steps {
        //         sh 'mvn validate'
        //     }
        // }
        // stage ('Maven Compile'){
        //     steps {
        //         sh 'mvn compile'
        //     }
        // }
        //  stage ('Maven Test'){
        //     steps {
        //         sh 'mvn test'
        //     }
        // }
        stage ('Maven Package'){
            steps {
                sh 'mvn package'
            }
          }
         }
       }
    //    stage('Sonar Analysis') {
    //         steps {
    //             script {
    //                 def scannerHome = tool 'sonar-scanner'
    //                 withSonarQubeEnv('sonar-server') {
    //                 sh """
    //                 ${scannerHome}/bin/sonar-scanner \
    //                 -Dsonar.organization=sudhakar527 \
    //                 -Dsonar.projectName=SpringBootPet \
    //                 -Dsonar.projectKey=sudhakar527_springbootpet \
    //                 -Dsonar.java.binaries=target
    //                 """
    //                 }
    //             }
    //         }
    //     }
        // stage("Quality Gate") {
        //     steps {
        //       timeout(time: 1, unit: 'MINUTES') {
        //         waitForQualityGate abortPipeline: true, credentialsId: 'sonar'
        //       }
        //     }
        // } 
        stage("Build Docker Image and TAG") {
            steps {
              script {
                sh 'docker build -t springboot:latest .'
              }
            }
        }  
    }
}       
