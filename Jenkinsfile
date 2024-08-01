pipeline {
    agent any

     environment {
        SSH_KEY = credentials('my-ssh-key') // Use the actual Jenkins SSH credential ID here
    }
    stages{
        stage('Build Maven'){
            steps{
                git url:'https://github.com/Ravipandey2308/star-agile-banking-finance.git', branch: "master"
               sh 'mvn clean install'
            }
        }
        stage('Build docker image'){
            steps{
                script{
                    sh 'docker build -t ravi6748/capstoneproject:v1 .'
                }
            }
        }
         stage('Docker login') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'Docker-cred', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                    sh "echo $PASS | docker login -u $USER --password-stdin"
                    sh 'docker push  ravi6748/capstoneproject:v1'
                }
            }
        }
         stage('Deploy to Test') {
            steps {
                ansiblePlaybook(
                    inventory: 'hosts',
                    playbook: 'ansible.yml'
                )
              //  ansiblePlaybook credentialsId: 'ansible-ssh-key', disableHostKeyChecking: true, installation: 'Ansible', inventory: '/etc/ansible/hosts/', playbook: 'ansible.yml', vaultTmpPath: ''
            }
     }
        
        // stage('Deploy with Ansible ') {
        //     steps {
        //         sshagent(credentials: ['my-ssh-key']) { // Use the actual Jenkins SSH credential ID here
        //             sh '''
        //             ansible-playbook -i inventory.ini ansible.yml --private-key $SSH_KEY
        //             '''
        //         }
        //     }
        // }
}
 post {
        success {
            echo 'Deployment succeeded.'
        }
        failure {
            echo 'Deployment failed.'
        }
    }
}
