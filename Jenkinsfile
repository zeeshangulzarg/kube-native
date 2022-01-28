pipeline {    

    agent any    
    
    environment {
        MSR_FQDN_PORT='mept85chvjetfnabn-os6fqmw9pvgl5rpif.labs.strigo.io:4443'
    }

    stages {
        stage('Build') {
            environment {
                MSR_ACCESS_KEY = credentials('jenkins-msr-access-token')
		MAJORMINOR = '0.0'
            }
            steps {
                sh 'docker --context=buildserver image build -t ${MSR_FQDN_PORT}/engineering/api-build:rc-${MAJORMINOR}.${BUILD_ID} api'
                sh 'docker --context=buildserver image build --target test -t ${MSR_FQDN_PORT}/engineering/api-build:unittest-${MAJORMINOR}.${BUILD_ID} api'
                sh 'docker --context=buildserver login -u jenkins -p ${MSR_ACCESS_KEY} ${MSR_FQDN_PORT}'
                sh 'docker --context=buildserver image push ${MSR_FQDN_PORT}/engineering/api-build:rc-${MAJORMINOR}.${BUILD_ID}'
                sh 'docker --context=buildserver image push ${MSR_FQDN_PORT}/engineering/api-build:unittest-${MAJORMINOR}.${BUILD_ID}'
            }
        }
    }
    
    post {
        always{
	        sh 'rm -rf ${WORKSPACE}/*'
        }
    }
}
