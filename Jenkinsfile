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
        stage('snyk security') {
            steps {
                withCredentials([string(credentialsId: 'snyk-token', variable: 'SNYK_TOKEN')]) {
                    sh '''
                    curl -fsSL https://static.snyk.io/cli/latest/snyk-linux -o snyk
                    chmod +x snyk
                    mkdir -p ~/.local/bin
                    mv snyk ~/.local/bin/
                    export PATH=$PATH:~/.local/bin
                    ~/.local/bin/snyk auth $SNYK_TOKEN
                    ~/.local/bin/snyk test --docker myapp --severity-threshold=medium || true
                    '''
                }
            }
        }
        stage('docker container') {
            steps {
                sh 'docker rm -f myappcontainer || true'
                sh 'docker run -d -p 80:80 --name myappcontainer myapp'
            }
        }
    }
}
