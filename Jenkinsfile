pipeline {
    agent any
    def mvnHome
    stages{
        stage('SCM-Checkout'){
            steps{
                git credentialsId: 'f33fe5fc-cd70-4969-a995-5a6d0ada26d5',  url: 'https://github.com/lavanyaVarry/MyFirstWebApp.git'
            }
        }
        stage ('Build'){
            steps{
                mvnHome = tool 'M3'
                sh "'${mvnHome}/bin/mvn' clean package"
            }
        }
    }
}