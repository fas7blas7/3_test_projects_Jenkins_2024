pipeline {
    agent any

    stages {
        stage("Checkout code") {
            //checkout the repository
            steps {
                git branch: 'main', url: 'https://github.com/fas7blas7/3_test_projects_Jenkins_2024'
            }           
        }        
    
        stage("Setup .net core") {
            //.net setup
            steps {
                bat '''
                echo Downloading .Net 6 SDK
                curl -l -o dotnet-sdk-6.0.424-win-x64.exe https://download.visualstudio.microsoft.com/download/pr/23c7bf0d-e22d-4372-bcb2-292eb36a5238/11af494be409759f46b679ab22e65a58/dotnet-sdk-6.0.424-win-x64.exe
                echo installing dotnet-sdk-6.0.424-win-x64.exe
                dotnet-sdk-6.0.424-win-x64.exe /quiet /norestart
                '''
            }
        }
    

        stage("Restoring nuget packages") {
            //restore nuget packages
            steps {
                bat 'dotnet restore SeleniumBasicExercise.sln'
            }
        }

        stage("Build") {
            //preparing build
            steps {
                bat 'dotnet build SeleniumBasicExercise.sln --configuration Release'
            }        
        }

        stage("Run tests Calculator-Project1") {
            //Run the tests
            steps {
                bat 'dotnet test TestProject1/TestProject1.csproj --logger "trx;LogFileName=TestResults.trx"'
            }
        }

                stage("Run tests Table -Project2") {
            //Run the tests
            steps {
                bat 'dotnet test TestProject2/TestProject2.csproj --logger "trx;LogFileName=TestResults.trx"'
            }
        }

                stage("Run tests Dropdown manipulations-Project3") {
            //Run the tests
            steps {
                bat 'dotnet test TestProject3/TestProject3.csproj --logger "trx;LogFileName=TestResults.trx"'
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