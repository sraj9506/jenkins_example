pipeline {
    agent any
    environment{
        DOCKER_CREDENTIALS_ID='docker_credentials'
    }
    stages {
        stage('Checkout')
        {
            steps
            {
                git branch: 'main', url: 'https://github.com/sraj9506/jenkins_example'
            } 
        }
        stage('Build')
        {
            steps
            {
                script
                {
                    dockerImage=docker.build("${params.docker_images}:${env.BUILD_NUMBER}")
                }
            }
        }
        stage('Push Docker Image')
        {
            steps
            {
                script
                {
                    docker.withRegistry("","${DOCKER_CREDENTIALS_ID}")
                    {
                        dockerImage.push()
                    }
                }
            }
        }
        stage('Cleanup')
        {
            steps
            {
                script
                {
                    sh "docker rmi ${params.docker_images}:${env.BUILD_NUMBER}"
                }
            }
        }
    }
}
