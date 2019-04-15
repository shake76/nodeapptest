node {
    
	

    env.AWS_ECR_LOGIN=true
    def newApp
    def registry = '161663572982.dkr.ecr.us-west-2.amazonaws.com/dev-jks-slaves'
    def registryCredential = ''
	
	stage('Git') {
		git 'https://github.com/shake76/nodeapptest'
	}
	stage('Build') {
		sh 'npm install'
	}
	stage('Test') {
		sh 'npm test'
	}
	stage('Building image') {
        docker.withRegistry( 'https://' + registry, registryCredential ) {
		    def buildName = registry + ":$BUILD_NUMBER"
			newApp = docker.build buildName
			newApp.push()
        }
	}
	stage('Registring image') {
        docker.withRegistry( 'https://' + registry, registryCredential ) {
    		newApp.push 'latest2'
        }
	}
    stage('Removing image') {
        sh "docker rmi $registry:$BUILD_NUMBER"
        sh "docker rmi $registry:latest"
    }
    
}
