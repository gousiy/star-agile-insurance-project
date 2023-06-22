def containerName="insure_me"
def tag="latest"
def dockerHubUser="alishataj"
def http="8084"


node{
	stage('checkout'){
	    checkout scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/gousiy/star-agile-insurance-project.git']])
    }

	stage('Build'){
	     echo "Cleaning... Compiling...Testing... Packaging..."
	     sh "mvn clean install"
    }

	stage("Image Prune"){
	   sh "docker image prune -a -f"
    }

	stage("Image Build"){
             sh "docker build -t $containerName:$tag --no-cache ."
	         echo "Image build complete"
    }

	stage('Push to Docker Registry') {
        withCredentials([usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'dockerUser', passwordVariable: 'dockerPassword')]) {
            sh "docker login -u $dockerUser -p $dockerPassword"
            sh "docker tag $containerName:$tag $dockerUser/$containerName:$tag"
            sh "docker push $dockerUser/$containerName:$tag"
            echo "Image push complete"
        }
   }
	stage("ansible") {
	ansiblePlaybook credentialsId: 'jenkins', installation: 'Ansible', inventory: '/etc/ansible/hosts', playbook: 'ansible-playbook.yml'
   }
}
	

