node{
    stage('Git checkout'){
        git branch: 'main', url: 'https://github.com/harish343/Kubernetes_delpoy.git' 
    }
    stage('sending file to ansible server'){
        sshagent(['ansible']) {
        sh 'ssh -o StrictHostKeyChecking=no ubuntu@44.199.231.118'
        sh 'scp /var/lib/jenkins/workspace/first_pipeline/* ubuntu@44.199.231.118:/home/ubuntu'
        
}
}
 stage('Docker build Image'){
        sshagent(['ansible']) {
        sh 'ssh -o StrictHostKeyChecking=no ubuntu@44.199.231.118 cd /home/ubuntu'
        sh 'ssh -o StrictHostKeyChecking=no ubuntu@44.199.231.118 docker image build -t $JOB_NAME:v1.$BUILD_ID .'
        
}
    }
    stage('Docker image build'){
          sshagent(['ansible']) {
        sh 'ssh -o StrictHostKeyChecking=no ubuntu@44.199.231.118 cd /home/ubuntu'
        sh 'ssh -o StrictHostKeyChecking=no ubuntu@44.199.231.118 docker image tag $JOB_NAME:v1.$BUILD_ID babbalrai/$JOB_NAME:v1.$BUILD_ID '
        sh 'ssh -o StrictHostKeyChecking=no ubuntu@44.199.231.118 docker image tag $JOB_NAME:v1.$BUILD_ID babbalrai/$JOB_NAME:latest '
        
}
    }
     stage('push docker image to docker hub'){
        sshagent(['ansible']) {
        withCredentials([string(credentialsId: 'dockerhub_passwd', variable: 'dockerhub_passwd')]) {
        sh 'ssh -o StrictHostKeyChecking=no ubuntu@44.199.231.118 docker login -u babbalrai -p ${dockerhub_passwd}'
        sh 'ssh -o StrictHostKeyChecking=no ubuntu@44.199.231.118 docker image push babbalrai/$JOB_NAME:v1.$BUILD_ID '
        
        sh 'ssh -o StrictHostKeyChecking=no ubuntu@44.199.231.118 docker image push babbalrai/$JOB_NAME:latest '
}
        
}
    }
    
}
