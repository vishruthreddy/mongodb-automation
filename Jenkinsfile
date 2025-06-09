pipeline {
    agent any
    parameters {
        string(name: 'RESTORE_DATE', defaultValue: '2025-06-04', description: 'Date to restore MongoDB backup from')
    }

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

        // stage("Enable authentication for all mongo components") {
        //     steps {
        //         script {
        //             withCredentials([string(credentialsId: 'root_pass', variable: 'BECOME_PASS')]) {
        //                 ansiblePlaybook credentialsId: 'Ansible',
        //                                 disableHostKeyChecking: true,
        //                                 installation: 'Ansible',
        //                                 inventory: 'dev.inv',
        //                                 playbook: 'Playbook/enable-auth.yml',
        //                                 extras: "-e ansible_become_pass=${BECOME_PASS}"
        //             }
        //         }
        //     }
        // }
        stage("Copy Mongo Scripts") {
            steps {
                script {
                    withCredentials([string(credentialsId: 'root_pass', variable: 'BECOME_PASS')]) {
                        ansiblePlaybook credentialsId: 'Ansible',
                                        disableHostKeyChecking: true,
                                        installation: 'Ansible',
                                        inventory: 'dev.inv',
                                        playbook: 'Playbook/copy-scripts.yml',
                                        extras: "-e ansible_become_pass=${BECOME_PASS}"
                    }
                }
            }
        }

        stage("Run Sharding Script") {
            steps {
                script {
                    withCredentials([string(credentialsId: 'root_pass', variable: 'BECOME_PASS')]) {
                        ansiblePlaybook credentialsId: 'Ansible',
                                        disableHostKeyChecking: true,
                                        installation: 'Ansible',
                                        inventory: 'dev.inv',
                                        playbook: 'Playbook/shardsetup.yml',
                                        extras: "-e ansible_become_pass=${BECOME_PASS}"
                    }
                }
            }
        }

        stage("Run Backup Script") {
            steps {
                script {
                    withCredentials([string(credentialsId: 'root_pass', variable: 'BECOME_PASS')]) {
                        ansiblePlaybook credentialsId: 'Ansible',
                                        disableHostKeyChecking: true,
                                        installation: 'Ansible',
                                        inventory: 'dev.inv',
                                        playbook: 'Playbook/mongobackup.yml',
                                        extras: "-e ansible_become_pass=${BECOME_PASS}"
                    }
                }
            }
        }

        stage("Run Restore Script") {
            steps {
                input message: "Proceed to restore MongoDB backup from ${params.RESTORE_DATE}?", ok: "Yes"
                script {
                    withCredentials([string(credentialsId: 'root_pass', variable: 'BECOME_PASS')]) {
                        ansiblePlaybook credentialsId: 'Ansible',
                                        disableHostKeyChecking: true,
                                        installation: 'Ansible',
                                        inventory: 'dev.inv',
                                        playbook: 'Playbook/mongorestore.yml',
                                        extras: "-e ansible_become_pass=${BECOME_PASS} -e date=${params.RESTORE_DATE}"
                    }
                }
            }
        }
    }
}
