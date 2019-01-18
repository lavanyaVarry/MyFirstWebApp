node{
        stage('SCM-Checkout'){
        git credentialsId: 'git-credentials', url: 'https://github.com/lavanyaVarry/MyFirstWebApp.git'        
        }
        stage('build'){
            def mvn_home = tool name: 'M3', type: 'maven'
            sh "'${mvn_home}/bin/mvn' clean package"
        }
        stage('build-docker-image'){
            sh "docker build -t sailavanya/mydockerhub:webappFrmJenkins ."
        }
        stage('push-docker-image'){
          withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'pwd', usernameVariable: 'username')]) {
                sh "docker login -u '${username}' -p '${pwd}'"
                sh "docker push sailavanya/mydockerhub:webappFrmJenkins"
            }
        }
        stage('deploy'){
            def dockerrun = 'docker run -p 8080:8080 -d --name MyFirstWebapp sailavanya/mydockerhub:webappFrmJenkins'
            sshagent(['webappserver']) {
                    sh "ssh -o StrictHostKeyChecking=no webserver@23.99.191.12 ${dockerrun}"
            }
        }
}