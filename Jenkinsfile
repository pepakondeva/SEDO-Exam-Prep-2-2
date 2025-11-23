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
                sh 'wget https://packages.microsoft.com/config/ubuntu/22.04/packages-microsoft-prod.deb -O packages-microsoft-prod.deb'
                sh 'sudo dpkg -i packages-microsoft-prod.deb'
                sh 'sudo apt-get update'
                sh 'sudo apt-get install -y dotnet-sdk-6.0'
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
