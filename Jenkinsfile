def BRANCH_DEV = 'develop'
def BRANCH_PROD = 'master'
pipeline {
    agent any
    stages {
        stage('Update Project') {
            steps {
                script {
                    if (BRANCH_NAME == BRANCH_PROD) {
                        sshPublisher(
                            publishers: [
                                sshPublisherDesc(
                                    configName: 'production',
                                    verbose: false,
                                    transfers: [
                                        sshTransfer(
                                            execCommand: "git pull origin master",
                                            execTimeout: 120000,
                                        )
                                    ]
                                )
                            ]
                        )
                    } else if (BRANCH_NAME == BRANCH_DEV) {
                        sshPublisher(
                            publishers: [
                                sshPublisherDesc(
                                    configName: 'development',
                                    verbose: false,
                                    transfers: [
                                        sshTransfer(
                                            execCommand: "git pull origin develop",
                                            execTimeout: 120000,
                                        )
                                    ]
                                ),
                            ]
                        )
                    }
                    
                }
            }
        }
        stage('Restart PM2 Project') {
            steps {
                script {
                    if (BRANCH_NAME == BRANCH_PROD) {
                        sshPublisher(
                            publishers: [
                                sshPublisherDesc(
                                    configName: 'production',
                                    verbose: false,
                                    transfers: [
                                        sshTransfer(
                                            execCommand: "sudo pm2 restart 0",
                                            execTimeout: 120000,
                                        )
                                    ]
                                )
                            ]
                        )
                    } if (BRANCH_NAME == BRANCH_DEV) {
                        sshPublisher(
                            publishers: [
                                sshPublisherDesc(
                                    configName: 'development',
                                    verbose: false,
                                    transfers: [
                                        sshTransfer(
                                            execCommand: "sudo pm2 restart 0",
                                            execTimeout: 120000,
                                        )
                                    ]
                                ),
                            ]
                        )
                    }
                }
            }
        }
    }
}