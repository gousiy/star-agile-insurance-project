def containerName="insure_me"
def tag="latest"
def dockerHubUser="dilipdil"
def http="8084"


node{
	stage('checkout'){
	    checkout scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/HarshithaAnand/star-agile-insurance-project.git']])
    }

	stage('Build'){
	     echo "Cleaning... Compiling...Testing... Packaging..."
	     sh "mvn clean install"
    }
    
    stage('publish test reports'){
       publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, keepAll: false, reportDir: '/var/lib/jenkins/workspace/Insure_Me_Project/target/surefire-reports', reportFiles: 'index.html', reportName: 'HTML Report', reportTitles: '', useWrapperFileDirectly: true])
       }

	stage("Image Prune"){
	   sh "docker image prune -a -f"
    }

	stage("Image Build"){
             sh "docker build -t $containerName:$tag --no-cache ."
	         echo "Image build complete"
    }

	stage('Push to Docker Registry') {
	   withCredentials([usernamePassword(credentialsId: 'DockerHub_Credentials', passwordVariable: 'dockerPassword', usernameVariable: 'dockerUser')]) {  
	        sh "docker login -u $dockerUser -p $dockerPassword"
            sh "docker tag $containerName:$tag $dockerUser/$containerName:$tag"
            sh "docker push $dockerUser/$containerName:$tag"
            echo "Image push complete"
        }
   }

}

