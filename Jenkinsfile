pipeline {
    agent any

    triggers {
        pollSCM '* * * * *'
    }
    tools{
        maven 'Maven'
    }

    environment {
        CI = false //do not treat errors as warnings
        SONARSCANNER = "SonarScanner"
    }

    
    stages {
        stage('Checkout') {
            steps {
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/Jay-Mat/wordsmith-api-project.git']])
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean install'
            }
        }

    }

        stage('Deployment') {
            steps {
                sh 'docker build -t word-smith-api'
                sh 'docker tag word-smith-api:${BUILD_NUMBER} jaymath237/wordsmith-project-api'
                sh 'docker login -u ${USERNAME} -p ${PASSWORD} docker.io'
                sh 'docker push  jaymath237/word-smith-api'
            }
        }

    }


