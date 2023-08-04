pipeline {

  environment {
    dockerimagename = "madhan1098/test-repo"
    dockerImage = ""
	dockerHome = tool 'myDocker'
	TEST="${env.WORKSPACE}/build-dir:${env.PATH}"
  }

  agent any

  stages {

    stage('Checkout Source') {
      steps {
		git branch: 'main', url: 'https://github.com/madhan1098/docker-sample.git'
      }
    }
	stage('Initialize') {
      steps {
		echo "PATH is: $PATH"
      }
    }	
    stage('Build image') {
      steps{
        script {
          dockerImage = docker.build dockerimagename
        }
      }
    }

    stage('Pushing Image') {
      environment {
               registryCredential = 'dockerhub-credentials'
           }
      steps{
        script {
          docker.withRegistry( 'https://registry.hub.docker.com', registryCredential ) {
            dockerImage.push("latest")
          }
        }
      }
    }

    stage('Deploying React.js container to Kubernetes') {
      steps {
        script {
          kubernetesDeploy(configs: "deployment.yaml", "service.yaml")
        }
      }
    }

  }

}