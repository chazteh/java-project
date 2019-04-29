properties([pipelineTriggers([githubPush()])]) 
node('linux'){
    stage('Build'){
       git 'https://github.com/chazteh/java-project.git'
       sh "ant -f build.xml -v"
    }
    stage('Test'){
        sh 'ant -f test.xml -v'
    }
    stage('Reports'){
        junit 'reports/*.xml'
    }
    stage ('Deploy'){  
	    withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', accessKeyVariable: 'AWS_ACCESS_KEY_ID', credentialsId: 'AWS user for HW10', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY']]) {
        sh 'aws cloudformation describe-stack-resources --stack-name SEIS665Spring2019-01-HW10 --region us-east-1' 
	sh 'aws s3 cp /workspace/java-pipeline/dist/rectangle-${BUILD_NUMBER}.jar s3://cf-templates-yjmoozs2jef4-us-east-1/'
        }	    
		
	}
}
