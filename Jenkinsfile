def BRANCH_DEV = 'develop'
def BRANCH_PROD = 'master'
def APP = 'express-admin-arif'
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
                                    verbose: true,
                                    transfers: [
                                        sshTransfer(
                                            execCommand: "cd ${APP} | git checkout master | git pull origin master",
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
                                    verbose: true,
                                    transfers: [
                                        sshTransfer(
                                            execCommand: "cd ${APP} | git checkout develop | git pull origin develop",
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
        stage('Install Package') {
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
                                            execCommand: "cd ${APP} | npm install;",
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
                                            execCommand: "cd ${APP} | npm install;",
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
                                            execCommand: "cd ${APP} | sudo pm2 stop app.js | sudo pm2 start app.js;",
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
                                            execCommand: "cd ${APP} | sudo pm2 stop app.js | sudo pm2 start app.js;",
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