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
                powershell '''
                [Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12
                $installer = "dotnet-sdk.exe"
                Invoke-WebRequest -Uri "https://download.visualstudio.microsoft.com/download/pr/3c1d94f2-6a12-462a-9f4b-4c39f9a0d21d/90b1976ae74d58968d8c369cdb8f8575/dotnet-sdk-6.0.136-win-x64.exe" -OutFile $installer
                Start-Process -FilePath ".\\$installer" -ArgumentList "/quiet", "/norestart" -Wait
                
                # Manually add to PATH in this session (default .NET install path)
                $dotnetPath = "C:\\Program Files\\dotnet"
                $env:Path += ";$dotnetPath"
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
                bat 'dotnet build SeleniumIde.sln --no-restore'
            }
        }
        
        stage('Run the tests') {
            steps {
                bat 'dotnet test SeleniumIde.sln --no-build --logger "trx;LogFileName=TestResults.trx"'
            }
        }
    }
    
    post {
        always {
            // Archive all possible test result files
            archiveArtifacts artifacts: '**/TestResults/**/*.trx', allowEmptyArchive: true
            
            // Publish MSTest results
            step([
                $class: 'MSTestPublisher',
                testResultsFile: '**/TestResults/**/*.trx'
            ])
        }
    }
}