pipeline{
	agent any
	
	stages{
		stage('Checkout code'){
			steps{
				git branch: 'main', url: 'https://github.com/YasenKirilovORH/Workshop-CI-System-with-Selenium-Appium-Tests-II-Task1'
			}
		}
		
		stage('Set up .NET Core') {
			steps {
				powershell '''
				[Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12
				Invoke-WebRequest -Uri "https://download.visualstudio.microsoft.com/download/pr/3c1d94f2-6a12-462a-9f4b-4c39f9a0d21d/90b1976ae74d58968d8c369cdb8f8575/dotnet-sdk-6.0.136-win-x64.exe" -OutFile "dotnet-sdk.exe"
				Start-Process -FilePath ".\\dotnet-sdk.exe" -ArgumentList "/quiet", "/norestart" -Wait
				'''
    }
}
		
		stage('Install nuget packages'){
			steps{
				bat 'dotnet restore SeleniumIde.sln'
			}
		}
		
		stage('Build the project'){
			steps{
				bat 'dotnet build SeleniumIde.sln'
			}
		}
		
		stage('Run the tests'){
			steps{
				bat 'dotnet test SeleniumIde.sln --logger "trx; LogFileName=TestResults.trx"'
			}
		}
	}
	
	post{
		always{
			archiveArtifacts artifacts: '**/TestResults/*.trx',
			allowEmptyArchive: true
			step([
				$class: 'MSTestPublisher',
				testResultsFile: '**/TestResults/*.trx'
			])
		}
	}
}