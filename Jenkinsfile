node {
    def mvnHome = tool 'maven-3.5.2'
    def dockerImageTag = "devopsexample${env.BUILD_NUMBER}"

    stage('Clone Repo') {
        git branch: 'main',
            url: 'https://github.com/Afsar486/CI-CD-java-maven-docker.git'
    }

    stage('Build Project') {
        sh "'${mvnHome}/bin/mvn' clean install"
    }

    stage('Build Docker Image') {
        sh "docker build -t devopsexample:${env.BUILD_NUMBER} ."
    }

    stage('Docker Login & Push') {
        withCredentials([string(credentialsId: 'dockerhub-pwd', variable: 'DOCKER_PWD')]) {
            sh "echo ${DOCKER_PWD} | docker login -u afsar486 --password-stdin"
            sh "docker tag devopsexample:${env.BUILD_NUMBER} afsar486/myapplication:${env.BUILD_NUMBER}"
            sh "docker push afsar486/myapplication:${env.BUILD_NUMBER}"
        }
    }
}
