node(){
       stage("sourcecode from git"){
        git branch: 'main', url: 'https://github.com/kumarchakradhar/springwithmongo.git'
    }
    stage("maven build"){
      def mavenHome =  tool name: "M2_HOME", type: "maven"
      def mavenCMD = "${mavenHome}/bin/mvn"
      sh "${mavenCMD} clean package"
    }
    stage("build docker image"){
        sh 'docker build -t bayatapalli/springmongo-pipe:${BUILD_NUMBER} .'
    }
    stage("push docker image"){
        withCredentials([string(credentialsId: 'DOCKERPWD', variable: 'DOCKERPWD')]) {
    sh "docker login -u bayatapalli -p ${DOCKERPWD}"
     }
     sh "docker push bayatapalli/springmongo-pipe:${BUILD_NUMBER}"
    }
    stage("copy manifest file"){
        sshagent(['e9db2074-9792-4198-a8d1-b4de70b5739e']) {
        sh "scp -o StrictHostKeyChecking=no springBootMongo.yml ubuntu@13.234.37.243:/home/ubuntu" 
        sh "ssh ubuntu@13.234.37.243 kubectl apply -f springBootMongo.yml"
     }
    }
}
