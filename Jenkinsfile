stage 'Clear workspace and Access repository'
node {
    cleanWs()   //Clean the workspace
    git branch: 'modify',
            url: 'https://github.com/ColmCharlton/SystemInfo'

}

stage 'Setup virtual environment'
node {
// Creates the virtualenv before proceeding
    withPythonEnv('Python3') {


        if (isUnix()) {
            //"Linux"
            stage 'Install dependencies'
            node {
                sh 'pip install nose'
                sh 'pip install coverage '
            }
            stage 'Run tests and code coverage'
            node {

                sh 'nosetests'
                sh 'coverage run LinuxCommands.py'
                //sh 'coverage run JsonEdit.py'
                // sh 'coverage run oScommands.py'
                sh 'coverage html'
            }
        } else {
            //"Windows"
            stage 'Install dependencies'
            node {
                bat label: '', script: 'pip install nose'
                bat label: '', script: 'pip install coverage'

            }
            stage 'Run tests and code coverage'
            node {

                bat label: '', script: 'nosetests'
                bat label: '', script: 'coverage run WindowsCommands.py'
                //bat label: '', script: 'coverage run JsonEdit.py'
                //bat label: '', script: 'coverage run oScommands.py'
                bat label: '', script: 'coverage html'
            }
            stage('Archival')
                node {
//                    publishHTML('windows')
                    publishHTML([allowMissing         : true,
                                 alwaysLinkToLastBuild: false,
                                 keepAll              : true,
                                 reportDir            : 'htmlcov',
                                 reportFiles          : 'index.html',
                                 reportName           : 'Code Coverage',
                                 reportTitles         : ''])

                    archiveArtifacts allowEmptyArchive: true, artifacts: 'windows.json'
                }
                stage 'Notify user'
                node {
                    notify 'Run successfully'
                }
            }

        }
    }

//    stage('Archival') {
//        node{
//        publishHTML([allowMissing         : true,
//                     alwaysLinkToLastBuild: false,
//                     keepAll              : true,
//                     reportDir            : 'htmlcov',
//                     reportFiles          : 'index.html',
//                     reportName           : 'Code Coverage',
//                     reportTitles         : ''])
//
//        archiveArtifacts allowEmptyArchive: true, artifacts: '*.json'
//    }
//    }
//
//
//    stage 'Notify user'
//    node {
//        notify 'Run successfully'
//    }


def notify(status) {
    //Notify method sends details to email
    emailext(
            to: "columcharlton@gmail.com",
            subject: "${status}: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
            body: """<p>${status}: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]':</p>
        <p>Check console output at <a href='${env.BUILD_URL}'>${env.JOB_NAME} [${env.BUILD_NUMBER}]</a></p>""",
    )
}

def publishHTML(file) {
publishHTML([allowMissing         : true,
             alwaysLinkToLastBuild: false,
             keepAll              : true,
             reportDir            : 'htmlcov',
             reportFiles          : 'index.html',
             reportName           : 'Code Coverage',
             reportTitles         : ''])

archiveArtifacts allowEmptyArchive: true, artifacts: "${file}.json"
}
