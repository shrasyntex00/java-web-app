pipeline {
  agent any
    environment {
       AWS_ACCOUNT_ID= "803561623563"
       AWS_DEFAULT_REGION="ap-south-1"
       IMAGE_REPO_NAME= "ecrpipeline"
       IMAGE_TAG= "latest"
       REPOSITORY_URI= "${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com/${IMAGE_REPO_NAME}"
    }
    
    stages {
        stage('SCM Checkout') {
            steps {
                git 'https://github.com/shrasyntex00/java-web-app.git'
            } 
        }
        
        // stage('Unit Testing') {
        //     steps {
        //       sh 'mvn test'
        //     }
        // }
        
        // stage('Integration Testing'){
        //     steps {
        //         sh 'mvn verify -DskipUnitTests'
        //     }
        // }
        
        stage('Build') {
            steps {
              sh 'mvn clean package'
            }
        }

        stage('Upload war to Nexus'){
            steps {
                script{                                       
                    def readPomVersion = readMavenPom file: 'pom.xml'
                    // def nexusRepo = readPomVersion.version.endsWith("SNAPSHOT") ? "skan-snapshot" : "skanjob"
                    nexusArtifactUploader artifacts: [
                          [
                            artifactId: 'java-web-app',
                            classifier: '',
                            file: "target/java-web-app-${mavenPom.version}.war", 
                            type: 'war'
                          ]
                        ], 
                        credentialsId: 'Nexuscred', 
                        groupId: 'com.mt',
                        nexusUrl: '65.2.130.183:8081/', 
                        nexusVersion: 'nexus3',
                        protocol: 'http',
                        repository: 'test-release', 
                        version: "${mavenPom.version}"
                }
              }
        }
        
        // stage('SonarQube Analysis'){
        //     steps {
        //             withSonarQubeEnv(installationName: 'sonarqubeserver') {
        //               sh 'mvn clean package sonar:sonar'
        //             }  
                
        //     }
        // }
        
        // stage('Quality Gate Analysis'){
        //     steps{
        //             waitForQualityGate abortPipeline: true 
        //     }
        // }
        
        // stage('push nexus artifact'){
        //     steps {
        //         sh 'mvn clean deploy'
        //     }
        // }
        
        // stage('deploy to tomcat') {
        //     steps {
        //       deploy adapters: [tomcat9(credentialsId: 'cefc1c4e-fdf9-4f16-921d-c6cfe67330af', path: '', url: 'http://13.232.95.30:8082/')], contextPath: null, war: '**/*.war'
        //     }
        // }
        
        // stage('build docker image') {
        //     steps {
        //         script{
        //             dockerImage = docker.build "${IMAGE_REPO_NAME}:${IMAGE_TAG}"
        //         }
        //     }
        // }

        // stage('DockerHUB LOGIN & push image') {
        //     steps {
        //         withCredentials([string(credentialsId: 'dockerhub-credentials', variable: 'dockerhubcredentials')]) {
        //             sh "docker login -u account1996 -p ${dockerhubcredentials}"  
        //         }
        //             sh 'docker push account1996/java:1'
      	//   }
        // }
        
        // stage('Loggingto AWS ECR') {
        //     steps {
        //         sh "aws ecr get-login-password --region ap-south-1 | docker login --username AWS --password-stdin 803561623563.dkr.ecr.ap-south-1.amazonaws.com"
        //     }
        // }
        
        // stage('Pushingto ECR') {
        //   steps{
        //        sh "docker build -t ecrpipeline ."
        //        sh "docker tag ecrpipeline:latest 803561623563.dkr.ecr.ap-south-1.amazonaws.com/ecrpipeline:latest"
        //        sh "docker push 803561623563.dkr.ecr.ap-south-1.amazonaws.com/ecrpipeline:latest"
        //     }
        // }
        
        // stage('K8S Deploy'){
        //     steps{
        //         // kubernetesDeploy(
        //         //   config: 'java-web-app-docker/javawebapp-deployment.yml',
        //         //   kubeconfigId:'K8S',
        //         //   enableConfigSubstitution: true
        //         // )
                
        //       sh 'kubectl apply -f javawebapp-deployment.yml'
        //     }
        // }
    }
}
 