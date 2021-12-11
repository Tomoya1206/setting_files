pipeline{
    agent any

    environment{
        CONFIG=""
    }
    
    stages{
        stage('load config'){
            steps{
                script{
                    CONFIG = readYaml(file: 'config.yml')
                }
            }
        }


        stage('src_clone'){
            steps{
                cleanWs()
                checkout([$class: 'GitSCM', branches: [[name: '*/develop']], extensions: [[$class: 'RelativeTargetDirectory', relativeTargetDir: 'src']], userRemoteConfigs: [[url: 'https://github.com/Tomoya1206/sandbox.git']]])
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [[$class: 'RelativeTargetDirectory', relativeTargetDir: 'artifact']], userRemoteConfigs: [[url: 'https://github.com/Tomoya1206/artifact.git']]])
            }
        }

        stage('build'){
            agent{
                docker{
                    image 'ubuntu:ccpp'
                    args "-v ${WORKSPACE}/src:/home/projects"
                    reuseNode true
                }
            }
            steps{
                sh"""
                cd src/
                gcc -o "${CONFIG.filename}".out hello.c
                ./"${CONFIG.filename}".out
                mv "${CONFIG.filename}".out ../artifact/
                cd ../
                """
            }
        }

        stage('commit&push'){
            steps{
                sh"""
                cd artifact/
                git checkout -b main
                git branch -a
                git add .
                git commit -m "add artifact"
                git remote set-url origin https://Tomoya1206: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@github.com/Tomoya1206/artifact.git
                git push -u origin main
                """
            }
        }            
    }

    // post{
    //     always{
    //         cleanWs()
    //     }
    // }
}
