pipeline {
    agent any

    triggers {
        pollSCM '* * * * *'
    }
    environment {
        CI = false //do not treat errors as warnings
        SONARSCANNER = "SonarScanner"
    }


        stage('Sonar Analysis') {
            environment {
                scannerHome = tool "SonarScanner";
            }
            steps {
                withSonarQubeEnv('SonarScanner') {
                sh "${scannerHome}/bin/sonar-scanner"         
}
            }
        }

        stage('Build') {
            steps {
                sh 'mvn package'
                sh 'mvn clean install'
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
