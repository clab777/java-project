properties([pipelineTriggers([githubPush()])]) 

node('linux'){
    stage('Test'){
        sh "ant -f test.xml -v"
        junit 'reports/result.xml'
    }
    stage('Build'){
        git 'https://github.com/clab777/java-project.git'
        sh "ant -f build.xml -v"
    }
    stage('Deploy'){
        sh 'aws s3 cp ${WORKSPACE}/dist/rectangle-${BUILD_NUMBER}.jar s3://seis665-clab-a10/'
    }
    stage('Report'){
        withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', accessKeyVariable: 'AWS_ACCESS_KEY_ID', credentialsId: 'AWSJenkinsUser', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY']]) {
            sh 'aws cloudformation describe-stack-resources --region us-east-1 --stack-name jenkins'
        }
    }
}
