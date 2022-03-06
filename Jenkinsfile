node {

 environment {
        DOCKERHUB_CREDENTIALS=credentials('dockerhub')
        def docker_img_name="annaliyx/flask-project"
    }
   //try{
        stage('Clone') {
           
                //clone source code
                checkout([$class: 'GitSCM', branches: [[name: "main"]], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: 'd5528b90-0f38-41b1-9f4d-3689a040e918', url: 'https://github.com/dilligence1991/flask.git']]])
           
        }
        stage ('Scantist') {
           
                 //scan code
         sh ' cd ${WORKSPACE} && cp /Users/liyaxing/.jenkins/workspace/scantist-bom-detect.jar . && java -jar ${WORKSPACE}/scantist-bom-detect.jar '
                
         
        }
        stage('Build') {
            
                
                sh 'sudo docker build -t ${docker_img_name}:latest .'
            
        }
        stage('Docker Build') {
            
                //create and deploy image to docker hub
                
                sh "docker tag ${docker_img_name}:${env.BUILD_NUMBER} ${docker_img_name}:latest"
                withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'dockerPassword', usernameVariable: 'dockerUser')]) {
                sh "docker login -u ${dockerUser} -p ${dockerPassword} hub.docker.com"
                sh "docker push ${docker_img_name}:latest"
                }
            
        }
        stage('Argocd Deploy') {
            
                //
                //sh 'docker push ${docker_img_name}:latest'
            
        }
    //}catch(err) {
     //  always {
     //    echo 'end'
     //  }
    //}
}
