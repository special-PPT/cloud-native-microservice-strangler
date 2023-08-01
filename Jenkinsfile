def strangler_images = ["legacy-edge","customer-service","edge-service","profile-web","discovery-service","config-service","user-service","profile-service"]

pipeline {
    agent {label 'workernode1'}
    

    environment {
        legacyedge =''
        customerservice = ''
        registryCredential = 'dockerhub_cred'

    }
    
    stages {
        stage('Checkout Git') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[credentialsId: 'github-credential-token', url: 'https://github.com/special-PPT/cloud-native-microservice-strangler-example.git']]])
            }
        }
        stage('Build') {
            steps {
                script {
				    sh "mvn clean install"
                }
            }
        }
        stage ('Tag Docker Image') {
            steps {
                script {
				    strangler_images.each { name->
				        sh "docker tag ${name}:latest robinwei/${name}"
                }
            }
        }

        }
        
        stage ('Upload Image') {
            steps {
                script {
                    docker.withRegistry( '', registryCredential ) {
                        strangler_images.each { name->
                            sh "docker push robinwei/${name}"
                        }
                    }
                }
            }
        }
  
    }
}
