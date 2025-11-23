pipeline {
    agent any

    environment {
        DOTNET_ROOT = "${WORKSPACE}/dotnet"
        PATH = "${WORKSPACE}/dotnet:${env.PATH}"
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install .NET 6 SDK') {
            steps {
                sh '''
                    curl -sSL https://dot.net/v1/dotnet-install.sh -o dotnet-install.sh
                    bash dotnet-install.sh --channel 6.0 --install-dir "$DOTNET_ROOT"
                '''
            }
        }

        stage('Restore') {
            steps {
                sh 'dotnet restore'
            }
        }

        stage('Build') {
            steps {
                sh 'dotnet build --no-restore'
            }
        }

        stage('Run Tests') {
            steps {
                sh '''
                    dotnet test \
                        --no-build \
                        --logger "trx;LogFileName=test_results.trx"
                '''
            }
        }
    }

    post {
        always {
            junit '**/test_results.trx'
        }
    }
}
