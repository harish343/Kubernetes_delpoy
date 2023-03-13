node{
    stage('Git checkout'){
        git branch: 'main', url: 'https://github.com/harish343/Kubernetes_delpoy.git' 
    }
    stage('sending file to ansible server'){
        sshagent(['ansible']) {
        sh 'ssh -o StrictHostKeyChecking=no ubuntu@44.203.24.217'
        sh 'scp /var/lib/jenkins/workspace/first_pipeline/* ubuntu@44.203.24.217:/home/ubuntu'
        
}
}
 stage('Docker build Image'){
        sshagent(['ansible']) {
        sh 'ssh -o StrictHostKeyChecking=no ubuntu@44.203.24.217 cd /home/ubuntu'
        sh 'ssh -o StrictHostKeyChecking=no ubuntu@44.203.24.217 docker image build -t $JOB_NAME:v1.$BUILD_ID .'
        
}
    }
    stage('Docker image build'){
          sshagent(['ansible']) {
        sh 'ssh -o StrictHostKeyChecking=no ubuntu@44.203.24.217 cd /home/ubuntu'
        sh 'ssh -o StrictHostKeyChecking=no ubuntu@44.203.24.217 docker image tag $JOB_NAME:v1.$BUILD_ID harish343/$JOB_NAME:v1.$BUILD_ID '
        sh 'ssh -o StrictHostKeyChecking=no ubuntu@44.203.24.217 docker image tag $JOB_NAME:v1.$BUILD_ID harish343/$JOB_NAME:latest '
        
}
    }
     stage('push docker image to docker hub'){
        sshagent(['ansible']) {
        withCredentials([string(credentialsId: 'dockerhub_passwd', variable: 'dockerhub_passwd')]) {
        sh 'ssh -o StrictHostKeyChecking=no ubuntu@44.203.24.217 docker login -u harish343 -p ${dockerhub_passwd}'
        sh 'ssh -o StrictHostKeyChecking=no ubuntu@44.203.24.217 docker image push harish343/$JOB_NAME:v1.$BUILD_ID '
        
        sh 'ssh -o StrictHostKeyChecking=no ubuntu@44.203.24.217 docker image push harish343/$JOB_NAME:latest '
        sh 'ssh -o StrictHostKeyChecking=no ubuntu@44.203.24.217 docker image rm harish343/$JOB_NAME:v1.$BUILD_ID harish343/$JOB_NAME:latest '
}
        
}
    }
       stage('copy files from ansible to kubernetes server'){
          sshagent(['kubernetes_server']) {
        sh 'ssh -o StrictHostKeyChecking=no ubuntu@44.201.111.15 '
        sh 'scp /var/lib/jenkins/workspace/first_pipeline/* ubuntu@44.201.111.15:/home/ubuntu'
     
        
}
    }
     stage('kubernetes depoyment using ansible'){
          sshagent(['ansible']) {
        sh 'ssh -o StrictHostKeyChecking=no ubuntu@44.203.24.217 cd /home/ubuntu '
        sh 'ssh -o StrictHostKeyChecking=no ubuntu@44.203.24.217 ansible-playbook ansible.yml -i inventory.txt'

     
        
}
    }
    
    
}
