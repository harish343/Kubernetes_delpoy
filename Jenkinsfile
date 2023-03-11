node{
    stage('Git checkout'){
        git branch: 'main', url: 'https://github.com/harish343/Kubernetes_delpoy.git' 
    }
    stage('sending file to ansible server'){
        sshagent(['ansible']) {
        sh 'ssh -o StrictHostKeyChecking=no ubuntu@34.205.247.219'
        sh 'scp /var/lib/jenkins/workspace/first_pipeline/* ubuntu@34.205.247.219:/home/ubuntu'
        
}
}
 stage('Docker build Image'){
        sshagent(['ansible']) {
        sh 'ssh -o StrictHostKeyChecking=no ubuntu@34.205.247.219 cd /home/ubuntu'
        sh 'ssh -o StrictHostKeyChecking=no ubuntu@34.205.247.219 docker image build -t $JOB_NAME:v1.$BUILD_ID .'
        
}
    }
    stage('Docker image build'){
          sshagent(['ansible']) {
        sh 'ssh -o StrictHostKeyChecking=no ubuntu@34.205.247.219 cd /home/ubuntu'
        sh 'ssh -o StrictHostKeyChecking=no ubuntu@34.205.247.219 docker image tag $JOB_NAME:v1.$BUILD_ID babbalrai/$JOB_NAME:v1.$BUILD_ID '
        sh 'ssh -o StrictHostKeyChecking=no ubuntu@34.205.247.219 docker image tag $JOB_NAME:v1.$BUILD_ID babbalrai/$JOB_NAME:latest '
        
}
    }
     stage('push docker image to docker hub'){
        sshagent(['ansible']) {
        withCredentials([string(credentialsId: 'dockerhub_passwd', variable: 'dockerhub_passwd')]) {
        sh 'ssh -o StrictHostKeyChecking=no ubuntu@34.205.247.219 docker login -u babbalrai -p ${dockerhub_passwd}'
        sh 'ssh -o StrictHostKeyChecking=no ubuntu@34.205.247.219 docker image push babbalrai/$JOB_NAME:v1.$BUILD_ID '
        
        sh 'ssh -o StrictHostKeyChecking=no ubuntu@34.205.247.219 docker image push babbalrai/$JOB_NAME:latest '
        sh 'ssh -o StrictHostKeyChecking=no ubuntu@34.205.247.219 docker image rm babbalrai/$JOB_NAME:v1.$BUILD_ID babbalrai/$JOB_NAME:latest '
}
        
}
    }
       stage('copy files from ansible to kubernetes server'){
          sshagent(['kubernetes_server']) {
        sh 'ssh -o StrictHostKeyChecking=no ec2-user@52.87.255.142 '
        sh 'scp /var/lib/jenkins/workspace/first_pipeline/* ec2-user@52.87.255.142:/home/ec2-user'
     
        
}
    }
     stage('kubernetes depoyment using ansible'){
          sshagent(['ansible']) {
        sh 'ssh -o StrictHostKeyChecking=no ubuntu@34.205.247.219 cd /home/ubuntu '
        sh 'ssh -o StrictHostKeyChecking=no ubuntu@34.205.247.219 ansible-playbook ansible.yml -i inventory.txt'
     
        
}
    }
    
    
}
