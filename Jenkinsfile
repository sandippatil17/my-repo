pipeline {
    agent any

    stages {
        stage('git config') {
            steps {
                git url:'https://github.com/sandippatil17/my-repo.git',branch:'main'
            }
        }
        stage('docker image build') {
            steps {
                sh 'docker build -t myapp .'
            }
        }
        stage('docker container') {
            steps {
                sh 'docker run -d myappcontainer -p 80:80 myapp'
            }
        }
    }
}
