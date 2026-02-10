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
        stage("Trivy Scan") {
            steps {
              script {
                sh 'trivy image --format table --scanners vuln -o trivy-image-report.html springboot:latest'
              }
            }
        }
        stage("Push Docker Image to AWS ECR") {
            steps {
              script {
                sh 'aws ecr get-login-password --region us-east-2 | docker login --username AWS --password-stdin 405894865527.dkr.ecr.us-east-2.amazonaws.com'
                sh 'docker tag springboot:latest 405894865527.dkr.ecr.us-east-2.amazonaws.com/myrepo:latest'
                sh 'docker push 405894865527.dkr.ecr.us-east-2.amazonaws.com/myrepo:latest'
              }
            }
        }
        stage("Deploy To Kubernetes") {
            steps {
              script {
                sh 'aws eks update-kubeconfig --region us-east-1 --name eksdemo1'
                sh 'kubectl apply -f k8s/sprinboot-deployment.yaml'
              }
            }
        }
    }
}       
