pipeline {
    agent any
    parameters {
        booleanParam(name: 'RC', defaultValue: false, description: 'Is this a Release Candidate?')
    }
    environment {
        VERSION = "0.1.0"        
        VERSION_RC = "rc.2"
    }
    stages {
        stage('Audit tools') {                        
            steps {
                auditTools()
            }
        }
        stage('Build') {
            environment {
                VERSION_SUFFIX = getVersionSuffix()
            }
            steps {
              echo "Building version: ${VERSION} with suffix: ${VERSION_SUFFIX}"
              sh 'touch build.sh'
            }
        }
        stage('Unit Test') {
            steps {
                echo "this is new test"
              }
            }
        stage('Smoke Test') {
            steps {
              echo "this is smoke test"
            }
        }
        stage('Publish') {
            when {
                expression { return params.RC }
            } 
            steps {
                writeFile file: 'publish-results.txt', text: 'published' 
                archiveArtifacts 'publish-results.txt'
            }
        }
    }
}


String getVersionSuffix() {
    if (params.RC) {
        return env.VERSION_RC
    } else {
        return env.VERSION_RC + '+ci.' + env.BUILD_NUMBER
    }
}

void auditTools() {
    sh '''
        git version
        pwd
    '''
}