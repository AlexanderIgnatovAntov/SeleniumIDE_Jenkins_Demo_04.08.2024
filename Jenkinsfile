pipeline {
    agent any

    stages {
        stage("Checkout code") {
            //check the repo
            steps {
                git branch: 'main', url: 'https://github.com/AlexanderIgnatovAntov/SeleniumIDE_Jenkins_Demo_04.08.2024'
            }
        }
        stage("Set up .Net Core") {
            //install dot net
            steps {
                bat '''
                echo Downloading .Net 6 Sdk
                curl -l -o dotnet-sdk-6.0.132-win-x64.exe https://download.visualstudio.microsoft.com/download/pr/0c82e7e6-fdde-49f2-a69f-bd986aeafe1b/9ea7411a22e661fff0e61e56a466e484/dotnet-sdk-6.0.132-win-x64.exe
                echo Installing dotnet-sdk-6.0.132-win-x64.exe
                dotnet-sdk-6.0.132-win-x64.exe /quiet /norestart
                '''
            }
        }
        stage("Restore dependencies") {
            //install dependencies
            bat 'dotnet restore SeleniumIde.sln'
        }
        stage("Build") {
            //build
            bat 'dotnet build SeleniumIde.sln --configuration Release'
        }
        stage("Run Tests") {
            //run tests
            bat 'dotnet test SeleniumIde.sln --logger "trx;logFileName=TestResults.trx"'
        }
    }

    post {
        always {
            archiveArtifacts artifacts: '**/TestResults/*.trx', allowEmptyArchive: true
            step({
                $class: 'MSTestPublisher',
                testResultsFile: '**/TestResults/*.trx'
            })
        }
    }
}