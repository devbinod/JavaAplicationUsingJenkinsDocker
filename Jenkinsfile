node {
    stage('SCM Checkout') {
        git credentialsId: 'git-cred', url: 'https://github.com/pantbinod/JavaAplicationUsingJenkinsDocker.git'

    }
    stage('MVN Package') {
        def mvnHome = tool name: 'maven-3', type: 'maven'
        def mvnCMD ="${mvnHome}/bin/mvn"
        sh "${mvnCMD} clean package"
    }
    stage('Build docker image') {
       sh "docker build -t developerbinod/myweb:1.0.0 ."
    }

    stage('Push image to docker hub') {
        withCredentials([string(credentialsId: 'docker-pass', variable: 'dockerpwd')]) {
         sh "docker login -u developerbinod -p ${dockerpwd}"
   }
         sh "docker push developerbinod/myweb:1.0.0"

    }
    
     stage('Run container on Dev Server') {
              withCredentials([string(credentialsId: 'docker-pass', variable: 'dockerpwd')]) {
         sh "docker login -u developerbinod -p ${dockerpwd}"
   }
         
         def dockerRun ="docker run -p 8080:8080 -d developerbinod/myweb:1.0.0"
        sshagent(['dev-server']) {
                sh "ssh -o StrictHostKeyChecking=no ubuntu@54.86.30.148 ${dockerRun}"

}

    }
}