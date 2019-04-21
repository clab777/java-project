properties([pipelineTriggers([githubPush()])]) 

node('linux'){
    stage('Unit Tests'){
        // this stage run JUnit tests and generate a report
    }
    stage('Build'){
        // this stage clone the repo and build the artifacts
    }
    stage('Deploy'){
        // deploy stage will copy the build artifact into an S3 bucket
    }
    stage('Report'){
        // generate CF report
    }
}
