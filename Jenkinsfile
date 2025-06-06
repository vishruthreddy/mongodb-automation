
pipeline {
    agent any

    stages {
        // stage("github repo"){
        //     steps {
        //         git branch: 'main',credentialsId: 'gitcred',url: 'https://github.com/saitejat1907/mongodb-automation.git'  // Cloning your GitHub repo
        //     }
        // }
        // stage("Install Mongos") {
        //     steps {
        //         ansiblePlaybook credentialsId: 'Ansible',
        //                          disableHostKeyChecking: true,
        //                          installation: 'Ansible',
        //                          inventory: 'dev.inv',
        //                          playbook: 'Playbook/Mongoinstall.yml'
        //     }
        // }

        stage("setup Configserver") {
            steps {
                ansiblePlaybook credentialsId: 'Ansible',
                                 disableHostKeyChecking: true,
                                 installation: 'Ansible',
                                 inventory: 'dev.inv',
                                 playbook: 'Playbook/Configserver.yml'
            }
        }


        stage("Initiate Config Replica Set") {
            steps {
                ansiblePlaybook credentialsId: 'Ansible',
                                 disableHostKeyChecking: true,
                                 installation: 'Ansible',
                                 inventory: 'dev.inv',
                                 playbook: 'Playbook/initiate-config-replset.yml'
            }
        }
        stage("create and initiate rsShard1") {
            steps {
                ansiblePlaybook credentialsId: 'Ansible',
                                 disableHostKeyChecking: true,
                                 installation: 'Ansible',
                                 inventory: 'dev.inv',
                                 playbook: 'Playbook/rsShard1.yml'
            }
        }
        stage("create and initiate rsShard2") {
            steps {
                ansiblePlaybook credentialsId: 'Ansible',
                                 disableHostKeyChecking: true,
                                 installation: 'Ansible',
                                 inventory: 'dev.inv',
                                 playbook: 'Playbook/rsShard2.yml'
            }
        }
        stage("Start Mongos router and add shards") {
            steps {
                ansiblePlaybook credentialsId: 'Ansible',
                                 disableHostKeyChecking: true,
                                 installation: 'Ansible',
                                 inventory: 'dev.inv',
                                 playbook: 'Playbook/Mongos.yml'
            }
        }
    }
}
