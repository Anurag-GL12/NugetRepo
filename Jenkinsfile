pipeline {
    agent any

    tools {
        // Install the .NET SDK and NuGet CLI
        dotnet 'dotnet-sdk'
        nuget 'nuget-cli'
    }

    environment {
        NUGET_API_KEY = credentials('nuget_api_key')
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Restore') {
            steps {
                sh 'dotnet restore'
            }
        }

        stage('Build') {
            steps {
                sh 'dotnet build --configuration Release'
            }
        }

        stage('Pack') {
            steps {
                sh 'dotnet pack --configuration Release'
            }
        }

        stage('Push to NuGet') {
            steps {
                sh 'nuget push **/bin/Release/*.nupkg -ApiKey $NUGET_API_KEY -Source https://api.nuget.org/v3/index.json'
            }
        }
    }

    post {
        success {
            echo 'NuGet package created and deployed successfully.'
        }
        failure {
            echo 'There was a problem in creating or deploying the NuGet package.'
        }
    }
}
