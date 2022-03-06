node {

 environment {
        DOCKERHUB_CREDENTIALS=credentials('dockerhub')
        //def docker_img_name="annaliyx/flask-project"
    }
   //try{
        stage('init') {
            script{
              def dockerPath = tool 'docker' 
              env.PATH = "${dockerPath}/bin:${env.PATH}" 
            }
         }
        stage('Clone') {
           
                //clone source code
                checkout([$class: 'GitSCM', branches: [[name: "main"]], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: 'd5528b90-0f38-41b1-9f4d-3689a040e918', url: 'https://github.com/dilligence1991/flask.git']]])
           
        }
        stage ('Scantist') {
           
                 //scan code
         sh ' cd ${WORKSPACE} && cp /Users/liyaxing/.jenkins/workspace/scantist-bom-detect.jar . && java -jar ${WORKSPACE}/scantist-bom-detect.jar '
                
         
        }
        stage('Docker Build') {
            script{
                //create and deploy image to docker hub
                //def dockerImgName="flask-project"
                sh ' docker -v'
                sh ' cd ${WORKSPACE} && docker build -t flask-project . ' 
                withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'dockerPassword', usernameVariable: 'dockerUser')]) {
                 
                 sh ' docker login -u ${dockerUser} -p ${dockerPassword} https://registry.hub.docker.com'
                 echo '${BUILD_NUMBER}'
                 sh ' docker tag flask-project:"${BUILD_NUMBER}" flask-project:latest '
                 sh ' docker push flask-project:latest '
                }
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
