def projectName = "TEST Jenkins Project"//TODO
def environments = [
    dev3: '10.4.105.237',
    local: ''
]
def credentials = [
    //dev3: 'adminCred'//TODO
    dev3: 'wpass_lc'
]
def reportingMailOnError = 'admin@example.com'//TODO

def deployLocally() {
    def deployDestinationPath = input message: 'Specify absolute path to you local installation',
        ok: 'Continue',
        parameters: [string(defaultValue: '~/work/test_production_deploy/', description: 'Root path to install project', name: 'localInstallationPath')]

        sh "cp -rf ${env.WORKSPACE}/* ${deployDestinationPath}"
        sh "cd ${deployDestinationPath}src"
        dir("${deployDestinationPath}src") {
            def res = sh returnStdout: true, script: 'java Main'
            echo res
        }
        //sh "unzip hybris-all-${selectedEnvironment}.zip"
}


node {
    try {
        stage('Checkout code') {
            sh 'echo "Checking out code..."'
            checkout scm
        }

        stage('Prepare environment') {
            sh 'echo "Prepare environment..."'//TODO
        }

        stage('Build') {
            /*dir('./hybris/bin/platform') {
                sh '. ./setantenv.sh'
                sh 'ant clean all'
            }*/
            sh 'javac src/*.java'
        }

        stage('Analyse code') {
            sh 'echo "Analyse code..."'//TODO
        }

        stage('Test') {
            sh 'echo "Testing..."'//TODO
        }

        stage('Deploy') {
            sh 'echo "Deploying..."'
            def choises = environments.keySet().join('\n')
            selectedEnvironment = input id: 'EnvironmentSpecification', message: 'What environment do you want to deploy', ok: 'Continue', parameters: [choice(choices: choises, description: 'Target environment to be deployed to', name: 'selectedEnvironment')]
            //sh "ant package -Dbuild.environment=${selectedEvironment}"

            if (selectedEnvironment == 'local') {
                deployLocally()
            } else {
                //TODO
                def deployDestinationPath = environments[selectedEnvironment]
                /*withCredentials([usernameColonPassword(credentialsId: 'a82f0790-25e1-452b-bf27-0990169cc0a8', variable: 'srvCred')]) {
                    sh "scp -r ${env.WORKSPACE} "
                    sh "ssh ${srvCred}@"+environments[selectedEnvironment]
                }
                withCredentials([usernamePassword(credentialsId: credentials[selectedEnvironment], passwordVariable: 'srvPwd', usernameVariable: 'srvUsr')]) {
                    sh "scp -r ${env.WORKSPACE}/* ${srvUsr}:/home/vladimir_gromyak/tmp/"//TODO
                }*/


                //sh "scp ${env.WORKSPACE}/hybris/temp/hybris/hybrisServer/package/hybris-all-${selectedEnvironment}.zip ${deployDestinationPath}hybris-all-${selectedEnvironment}.zip"
                //TODO: connect to server
                //sh "cd ${deployDestinationPath}"
                //sh "unzip hybris-all-${selectedEnvironment}.zip"
            }
        }
    } catch (err) {
        emailext body: "${env.BRANCH_NAME} is broken on ${projectName}! This needs to be resolved ASAP!", subject: "${env.BRANCH_NAME} Build Failed", to: reportingMailOnError
        throw err
    } finally {
       //TODO report?
    }
}