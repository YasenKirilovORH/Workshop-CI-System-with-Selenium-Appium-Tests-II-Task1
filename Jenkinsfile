pipeline{
	agent any
	
	stages{
		stage('Checkout code'){
			steps{
				git branch: 'main', url: 'https://github.com/YasenKirilovORH/Workshop-CI-System-with-Selenium-Appium-Tests-II-Task1'
			}
		}
		
		stage('Set up .NET Core'){
			steps{
				bat '''
				echo Installing .NET SDK 6.0
				choco install dotnet-sdk -y --version=6.0.100
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