// simple-e-shop/Jenkinsfile

pipeline {
    agent any
    tools { nodejs 'NodeJS 22' } // [확인] 님의 NodeJS 버전으로 설정
    
    stages {
        stage('1. Install Dependencies') { steps { sh 'npm install' } }
        stage('2. Build Project') { steps { sh 'npm run build' } }

        stage('3. Deploy to Synology NAS') {
            steps {
                sh 'npm run build' // 재확인을 위해 한번 더 빌드
                // [주의] NAS 폴더가 /web/react-app으로 되어 있어야 합니다.
                sshPublisher(
                    publishers: [
                        sshPublisherDesc(
                            configName: 'My-Synology-NAS', 
                            transfers: [
                                sshTransfer(
                                    sourceFiles: 'build/**', 
                                    removePrefix: 'build', 
                                    remoteDirectory: '/web/eshop' 
                                )
                            ],
                            execCommand: 'echo "Deployment finished!"' 
                        )
                    ]
                )
            }
        }
    }
    post { always { deleteDir() } }
}