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
				// Add the test SDK and JUnit logger
				bat 'dotnet add SeleniumIDE/SeleniumIde.csproj package Microsoft.NET.Test.Sdk --version 17.9.0'
				bat 'dotnet add SeleniumIDE/SeleniumIde.csproj package JunitXml.TestLogger'

				// Run tests with the correct logger URI
				bat 'dotnet test SeleniumIde.sln --logger "junit;LogFilePath=test-results.xml"'
			}
		}
        
        stage('Post Actions') {
            steps {
                archiveArtifacts artifacts: '**/test-results.xml', allowEmptyArchive: true
                junit '**/test-results.xml'
            }
        }
    }
}
