node {
	def registry = "arjunvyadav/myapp"
	def registryCredential = "dockerhub"

        stage('Checkout') {
            git url: 'https://github.com/arjun-venkatesh1/registration-login-java-xml-maven-jsp-mysql.git', branch: 'master'
        }
        def app
        stage('Image') {
                app = docker.build("${registry}:${env.BUILD_NUMBER}")
       }
       stage('Docker push')
        {
            docker.withRegistry('https://registry.hub.docker.com', "${registryCredential}") {
		app.push()
                app.push('latest')
              }
       }
	   stage('Deployment to Kubernetes cluster') {
		sh "chmod +x changeTag.sh"
		sh "./changeTag.sh ${env.BUILD_NUMBER}"
		sh "rm app-deployment.yml"
			script{
				try{
					sh "kubectl apply -f ."
				}catch(error){
					sh "kubectl create -f ."
				}
			}
	   }
}
