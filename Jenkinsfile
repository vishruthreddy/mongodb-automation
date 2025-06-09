pipeline {
    agent any

    stages {
        // stage("github repo") {
        //     steps {
        //         git branch: 'main', credentialsId: 'gitcred', url: 'https://github.com/saitejat1907/mongodb-automation.git'
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

        stage("Setup Configserver") {
            steps {
                script {
                    withCredentials([string(credentialsId: 'root_pass', variable: 'BECOME_PASS')]) {
                        ansiblePlaybook credentialsId: 'Ansible',
                                        disableHostKeyChecking: true,
                                        installation: 'Ansible',
                                        inventory: 'dev.inv',
                                        playbook: 'Playbook/Configserver.yml',
                                        extras: "-e ansible_become_pass=${BECOME_PASS}"
                    }
                }
            }
        }

        stage("Initiate Config Replica Set") {
            steps {
                script {
                    withCredentials([string(credentialsId: 'root_pass', variable: 'BECOME_PASS')]) {
                        ansiblePlaybook credentialsId: 'Ansible',
                                        disableHostKeyChecking: true,
                                        installation: 'Ansible',
                                        inventory: 'dev.inv',
                                        playbook: 'Playbook/initiate-config-replset.yml',
                                        extras: "-e ansible_become_pass=${BECOME_PASS}"
                    }
                }
            }
        }

        stage("Create and Initiate rsShard1") {
            steps {
                script {
                    withCredentials([string(credentialsId: 'root_pass', variable: 'BECOME_PASS')]) {
                        ansiblePlaybook credentialsId: 'Ansible',
                                        disableHostKeyChecking: true,
                                        installation: 'Ansible',
                                        inventory: 'dev.inv',
                                        playbook: 'Playbook/rsShard1.yml',
                                        extras: "-e ansible_become_pass=${BECOME_PASS}"
                    }
                }
            }
        }

        stage("Create and Initiate rsShard2") {
            steps {
                script {
                    withCredentials([string(credentialsId: 'root_pass', variable: 'BECOME_PASS')]) {
                        ansiblePlaybook credentialsId: 'Ansible',
                                        disableHostKeyChecking: true,
                                        installation: 'Ansible',
                                        inventory: 'dev.inv',
                                        playbook: 'Playbook/rsShard2.yml',
                                        extras: "-e ansible_become_pass=${BECOME_PASS}"
                    }
                }
            }
        }

        stage("Start Mongos Router and Add Shards") {
            steps {
                script {
                    withCredentials([string(credentialsId: 'root_pass', variable: 'BECOME_PASS')]) {
                        ansiblePlaybook credentialsId: 'Ansible',
                                        disableHostKeyChecking: true,
                                        installation: 'Ansible',
                                        inventory: 'dev.inv',
                                        playbook: 'Playbook/Mongos.yml',
                                        extras: "-e ansible_become_pass=${BECOME_PASS}"
                    }
                }
            }
        }

        stage("Enable authentication for all mongo components") {
            steps {
                script {
                    withCredentials([string(credentialsId: 'root_pass', variable: 'BECOME_PASS')]) {
                        ansiblePlaybook credentialsId: 'Ansible',
                                        disableHostKeyChecking: true,
                                        installation: 'Ansible',
                                        inventory: 'dev.inv',
                                        playbook: 'Playbook/enable-auth.yml',
                                        extras: "-e ansible_become_pass=${BECOME_PASS}"
                    }
                }
            }
        }
    }
}
