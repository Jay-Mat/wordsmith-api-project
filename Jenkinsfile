pipeline {
    agent any

    triggers {
        pollSCM '* * * * *'
    }
    environment {
        CI = false //do not treat errors as warnings
        SONARSCANNER = "SonarScanner"
    }

    stages {
        stage('Build') {
            steps {
                echo 'Building..'
                sh 'npm install'
                sh 'npm run build'
                sh 'docker build -t qr-momo-1:${BUILD_NUMBER} .'
            }  
        }

        stage('Deployment') {
            steps {
                sh 'docker tag qr-momo-1:${BUILD_NUMBER} jaymath237/qr-momo-1'
                sh 'docker login -u ${USERNAME} -p ${PASSWORD} docker.io'
                sh 'docker push  jaymath237/qr-momo-1'
            }
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


}
}
