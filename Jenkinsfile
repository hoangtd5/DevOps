pipeline{
    //Directives
    agent any
    environment{
       ArtifactId = "FlaskApp"
       Version = "2"
       Name = "FlaskApp"
       GroupId = "PythonFlask"
    }
    stages {
        // Specify various stage with in stages

        // stage 1. Build
        stage ('Build'){
            steps {
                echo 'Build not need.Zip'
                sh 'zip -r main.zip main.py' //zip file main.py vao
            }
        }

        // Stage2 : Testing
        stage ('Test'){
            steps {
                echo 'testing......'

            }
        }
        // Send to Nexus
        stage ('Send to Nexus'){
            steps {
                nexusArtifactUploader artifacts: [[artifactId: 'main', classifier: '', file: 'main.zip', type: 'py']],
                 credentialsId: 'f5ab4393-67ad-4c43-b5f8-edb95b25030c', groupId: 'FlaskApp', nexusUrl: '192.168.24.239:8081', 
                 nexusVersion: 'nexus3', protocol: 'http', repository: 'DeployPython', version: '2'

            }
        }
        // Deploy to docker
        stage ('Deploy to docker'){
            steps {
                echo ' Deploy......'
                sshPublisher(publishers: [sshPublisherDesc(configName: 'Ansible_ControlNode', 
                transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: 'ansible-playbook /home/ansibleadmin/deploydocker.yaml -i /etc/ansible/hosts',
                execTimeout: 120000, flatten: false, 
                makeEmptyDirs: false, noDefaultExcludes: false, 
                patternSeparator: '[, ]+', remoteDirectory: '', 
                remoteDirectorySDF: false, removePrefix: '', sourceFiles: '')], 
                usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])

            }
        }

       




    }

}