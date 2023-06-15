def COLOR_MAP = [
    'FAILURE' : 'danger',
    'SUCCESS' : 'good'
    ]

pipeline {
agent any
stages {
    stage('Build') {
steps {
echo 'Cloning projet from GITHUB'
git branch: 'master', url: 'https://github.com/dhanunjayaakula/Demo.git'
            }
        }
    stage('Fetch Katalon') {
steps {
echo 'Cloning projet from GITHUB'
git branch: 'master', url: 'https://github.com/dhanunjayaakula/WatchYES'
            }
        }
        stage('Test') {
steps {
                dir('/var/lib/jenkins/workspace/YES_Demo'){

sh 'docker run -t --rm -v "$(pwd)":/tmp/project katalonstudio/katalon katalonc.sh -projectPath=/tmp/project -retry=0 -testSuitePath="Test Suites/YES" -browserType="Chrome" -executionProfile="default" -apiKey="7dd94b29-f400-4eb8-9347-99ec374fb097" --config -proxy.auth.option=NO_PROXY -proxy.system.option=NO_PROXY -proxy.system.applyToDesiredCapabilities=true'
}
        }
    }
    stage('Deploy') {
steps {
echo 'Code will be deployed if the tests are passed'
        }
}
post {
    always {
        archiveArtifacts artifacts: 'Reports/**/*.*', fingerprint: true
        junit 'Reports/*/YES/*/*.xml'

        echo 'Slack Notification'
            slackSend( channel: "#appbuilds-qa-automation",
            color: COLOR_MAP[currentBuild.currentResult], 
            message: "${currentBuild.currentResult}:\n Job ${env.JOB_NAME} \n Logs path: ${JENKINS_URL}/job/${JOB_NAME}/${BUILD_ID}/artifact/Reports/")
        }
    }
}