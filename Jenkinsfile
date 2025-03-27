pipeline {
    agent any

    stages {
        
        stage ('Zip File') {
            steps {
                sh 'zip -r ansible-${BUILD_NUMBER}.zip * --exclude=Jenkinsfile'
                sh 'ls -l'
                sh 'echo "testing- March 27th"'
            }
        }

        stage ('Upload to JFrog') {
            steps {
                sh 'curl -uadmin:cmVmdGtuOjAxOjE3NzQ1NzM1NDM6UGVQRVpRMzFmOUczNDFTR042VHB3NFBTTHox -T ansible-${BUILD_NUMBER}.zip \
                "http://3.210.203.254:8081/artifactory/ansible/ansible-${BUILD_NUMBER}.zip"'
            }
        }

        stage ('PUSH FILES VIA SSH Ansible') {
            steps {
                sh 'whoami'
                sh 'echo "Checking for files before push"'
                sh 'ls -l'
                sshPublisher(publishers: [sshPublisherDesc(configName: \
                'AnsibleServer', transfers: [sshTransfer(cleanRemote: false, \
                excludes: '', execCommand: 'ls', execTimeout: 120000, flatten: false, \
                makeEmptyDirs: false, noDefaultExcludes: false, \
                patternSeparator: '[, ]+', remoteDirectory: '.', \
                remoteDirectorySDF: false, removePrefix: '', \
                sourceFiles: 'ansible-${BUILD_NUMBER}.zip')], usePromotionTimestamp: false, \
                useWorkspaceInPromotion: false, verbose: false)])
            }

        }
    }

}