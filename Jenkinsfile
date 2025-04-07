pipeline {
    agent any
    
    stages {
        stage('Checkout code') {
            steps {
                git branch: 'main', url: 'https://github.com/YasenKirilovORH/Workshop-CI-System-with-Selenium-Appium-Tests-II-Task1'
            }
        }
        
        stage('Set up .NET Core') {
            steps {
                bat '''
                curl -l -o dotnet-sdk-6.0.136-win-x86.exe https://builds.dotnet.microsoft.com/dotnet/Sdk/6.0.136/dotnet-sdk-6.0.136-win-x86.exe
                dotnet-sdk-6.0.136-win-x86.exe /quiet /norestart
                '''
            }
        }
        
        stage('Install nuget packages') {
            steps {
                bat 'dotnet restore SeleniumIde.sln'
            }
        }
        
        stage('Build the project') {
            steps {
                bat 'dotnet build SeleniumIde.sln'
            }
        }
        
        stage('Run the tests') {
            steps {
                bat 'dotnet test SeleniumIde.sln --logger "trx; LogFileName=TestResults.trx"'
            }
        }
    }
    
    post {
        always {
            archiveArtifacts artifacts: '**/TestResults/*.trx', allowEmptyArchive: true
            step([
                $class: 'MSTestPublisher',
                testResultsFile: '**/TestResults/*.trx'
            ])
        }
    }
}