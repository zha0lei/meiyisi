pipeline {
    agent any      #指定在那台Jenkins节点上运行
    stages {
        stage('更新开始') {
            steps {
                echo '更新开始'
                sh 'printenv'
            }
        }
        stage('build-image') {
            steps {
                retry(2) { sh 'docker build . -t myblog:latest'}    #构建镜像
            }
        }
        stage('deploy') {
            steps {
                timeout(time: 1, unit: 'MINUTES') {sh "kubectl apply -f deploy/"     #创建deployment
                }
            }
        }
    }
}

#pipeline {
#    // parameters {
#    //     //string(defaultValue: 'develop', description: 'mm-test-discovery-server分支', name: 'Dep_Branch', trim: true)
#    //     //choice(choices: ['mm-test', 'mm-prod'], description: '', name: 'Build_env')
#    // }
#
#    agent { label 'master' }
#
#
#    tools {
#        jdk "jdk11"
#        maven "mvn363"
#    }
#
#    stages {
#        stage('pull_build'){
#            steps {
#                dir('huahua-liveplay'){
#                    git branch: '${Dep_Branch}', credentialsId: 'gitlab-root-auth', url: 'ssh://git@172.81.248.104:19999/flashwhale-social/huahua-liveplay.git'
#                }
#                script{
#                    sh """
#                    cd huahua-liveplay && mvn clean package install -e -U -Dmaven.test.skip=true --settings /var/jenkins_home/TOOLS/settings-hhjy-test.xml
#                    """
#                }
#            }
#        }
#        stage('docker_build') {
#            steps {
#                script{
#                    IMAGE_COMMON = sh(returnStdout: true, script: 'cd huahua-liveplay && git rev-parse --short HEAD').trim()
#                    IMAGE_DATE = sh(returnStdout: true, script: 'date "+%Y%m%d%H%M%S"').trim()
#                    IMAGE_TAG="${IMAGE_DATE}-${IMAGE_COMMON}"
#                    IMAGE_NAME="registry-vpc.cn-hangzhou.aliyuncs.com/hhjy/huahua-liveplay-test:${IMAGE_TAG}"
#                    sh """
#                    mv huahua-liveplay/huahua-liveplay-server/target/huahua-liveplay-server.jar .
#                    docker build -t ${IMAGE_NAME} .
#                    """
#                }
#                withCredentials([usernamePassword(credentialsId: 'djwl-docker-hub-auth', passwordVariable: 'dockerHubPassword', usernameVariable: 'dockerHubUser')]) {
#                    sh "docker login -u ${dockerHubUser} -p ${dockerHubPassword} registry-vpc.cn-hangzhou.aliyuncs.com"
#                    sh "docker push ${IMAGE_NAME}"
#                 }
#            }
#        }
#        stage('kube_deploy') {
#            steps {
#                timeout(time:3, unit:'MINUTES'){
#                    script{
#                        sh """
#                        sed -i 's/##image_tag##/${IMAGE_TAG}/' huahua-liveplay.yaml
#                        mv huahua-liveplay.yaml huahua-liveplay-${IMAGE_TAG}.yaml
#                        cat huahua-liveplay-${IMAGE_TAG}.yaml
#                        kubectl apply -f huahua-liveplay-${IMAGE_TAG}.yaml --record --kubeconfig /var/jenkins_home/TOOLS/flashwhale-kubeconfig
#                        kubectl rollout status deployment -n huahua-test huahua-liveplay-test --kubeconfig /var/jenkins_home/TOOLS/flashwhale-kubeconfig
#                        rm -rf huahua-liveplay-${IMAGE_TAG}.yaml
#                        """
#                    }
#                }
#            }
#        }
#    }
#    
#}
