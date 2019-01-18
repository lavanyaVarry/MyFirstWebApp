pipeline { 
    agent any 
    stages {
        stage('SCM-Checkout') { 
            steps { 
                git credentialsId: 'git-credentials', url: 'https://github.com/lavanyaVarry/MyFirstWebApp.git'  
            }
        }
        stage('Build'){
            def mvn_home = tool name: 'M3', type: 'maven'
            steps {
                sh "'${mvn_home}/bin/mvn' clean package"
            }
        }
        stage('Build-Docker-Image') {
            steps {
                sh "docker build -t sailavanya/mydockerhub:webappFrmJenkins ."
            }
        }
        stage('Push-Image-To-Hub'){
            steps{
                withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'pwd', usernameVariable: 'username')]) {
                    sh "docker login -u '${username}' -p '${pwd}'"
                    sh "docker push sailavanya/mydockerhub:webappFrmJenkins"
                }
            }
        }
        stage('Deploy-To-Remote-Server'){
            steps{
                def dockerrun = 'docker run -p 8080:8080 -d --name MyFirstWebapp sailavanya/mydockerhub:webappFrmJenkins'
                sshagent(['webappserver']) {
                    sh "ssh -o StrictHostKeyChecking=no webserver@23.99.191.12 ${dockerrun}"
                }
            }
        }
    }
}