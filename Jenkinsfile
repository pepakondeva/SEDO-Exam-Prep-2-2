pipeline {
    agent any

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
               bash dotnet-install.sh --channel 6.0 --install-dir $WORKSPACE/dotnet
               export PATH="$WORKSPACE/dotnet:$PATH"
               '''
           }
       }


        stage('Restore') {
            steps {
                sh '''
                export PATH="$WORKSPACE/dotnet:$PATH"
                dotnet restore
                '''
            }
        }


        stage('Build') {
            steps {
                sh 'dotnet build --no-restore'
            }
        }

        stage('Run Tests') {
            steps {
                sh 'dotnet test HouseRentingSystem.Tests/HouseRentingSystem.Tests.csproj --no-build'
            }
        }
    }

    post {
        always {
            junit '**/TestResults/*.xml'
        }
    }
}
