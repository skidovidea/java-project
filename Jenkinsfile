properties([pipelineTriggers([githubPush()])]) 
node('linux'){
    stage('Init'){
        git 'https://github.com/skidovidea/java-project.git'
        sh "ant"
    }
    
    stage('Unit Test'){
        sh "ant -f test.xml -v"
        junit 'reports/result.xml'
    }
    
    stage('Build'){
        sh "ant -f build.xml -v"
    }
    
    stage('Deploy'){
        sh "aws s3 cp /workspace/java-pipeline/dist/rectangle-*.jar s3://homework10-2019bucket"
    }
    stage('Report'){
        withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', accessKeyVariable: 'AWS_ACCESS_KEY_ID', credentialsId: 'AWS user for Jenkins', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY']]) {
            sh "aws cloudformation describe-stack-resources --region us-east-1 --stack-name jenkins-study"
        }
    }
}
