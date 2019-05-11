node('linux') {   
	stage('Unit Test') {    
		git 'https://github.com/rclc/java-project.git'
		sh 'ant -f test.xml -v'   
		junit 'reports/result.xml'
	}   
	stage('Build') {    
		sh 'ant -f build.xml -v'   
	}   
	stage('Deploy') {    
		sh 'aws s3 cp /workspace/java-pipeline/dist/rectangle-${BUILD_NUMBER}.jar s3://seis665-public/'
	}
	stage('Report') {
		withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', accessKeyVariable: 'AWS_ACCESS_KEY_ID', credentialsId: '3ebb66d0-ad43-4f0c-844d-5ff96bec2a41', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY']]) {
			sh 'aws cloudformation describe-stack-resources --region us-east-1 --stack-name jenkins'
		}
	}	
}
