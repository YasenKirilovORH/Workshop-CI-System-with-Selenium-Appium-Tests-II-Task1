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
                powershell -Command "Invoke-WebRequest -Uri 'https://builds.dotnet.microsoft.com/dotnet/Sdk/6.0.136/dotnet-sdk-6.0.136-win-x64.exe' -OutFile 'dotnet-sdk-6.0.136-win-x64.exe'"
                dotnet-sdk-6.0.136-win-x64.exe /quiet /norestart
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
                bat 'dir /s /b **\\TestResults\\*.trx'  // Debugging: check if the trx file exists
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