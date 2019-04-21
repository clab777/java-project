properties([pipelineTriggers([githubPush()])]) 

node('linux'){
    stage('Test'){
        // this stage run JUnit tests and generate a report
        sh "ant -f test.xml -v"
        junit 'reports/result.xml'
    }
    stage('Build'){
        // this stage clone the repo and build the artifacts
        git 'https://github.com/clab777/java-project.git'
        sh "ant -f build.xml -v"
    }
    stage('Deploy'){
        // deploy stage will copy the build artifact into an S3 bucket
        sh 'aws s3 cp ${WORKSPACE}/dist/rectangle-${BUILD_NUMBER}.jar s3://seis665-clab-a10/'
    }
    stage('Report'){
        withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', accessKeyVariable: 'AWS_ACCESS_KEY_ID', credentialsId: 'AWS user for Jenkins', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY']]) {
            // generate cloudformation report 
            sh 'aws cloudformation describe-stack-resources --region us-east-1 --stack-name jenkins'
        }
    }
}
