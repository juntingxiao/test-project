def setBuildStatus(String message, String state, String url) {
  step([
      $class: "GitHubCommitStatusSetter",
      reposSource: [$class: "ManuallyEnteredRepositorySource", url: url],
      contextSource: [$class: "ManuallyEnteredCommitContextSource", context: "ci/jenkins/build-status"],
      errorHandlers: [[$class: "ChangingBuildStatusErrorHandler", result: "UNSTABLE"]],
      statusResultSource: [ $class: "ConditionalStatusResultSource", results: [[$class: "AnyBuildResult", message: message, state: state]] ]
  ]);
}


pipeline {

   
    agent{
        kubernetes {
          cloud 'ns-agent' // 在jenkins中可以配置多个集群， 在这里指定集群名称
          yaml '''
apiVersion: v1
kind: Pod
spec:
  containers:
  - name: alpine
    image: alpine:latest
    command: ["sleep", "3600"]  # 示例命令，这里使用 sleep 命令来保持容器运行
    tty: true   

'''
        }
    } 


    stages {
      stage('init') {
            steps{
                script{
  step([
      $class: "GitHubCommitStatusSetter",
      reposSource: [$class: "ManuallyEnteredRepositorySource", url: "${env.GIT_URL}"],
      contextSource: [$class: "ManuallyEnteredCommitContextSource", context: "ci/jenkins/build-status"],
      errorHandlers: [[$class: "ChangingBuildStatusErrorHandler", result: "UNSTABLE"]],
      statusResultSource: [ $class: "ConditionalStatusResultSource", results: [[$class: "AnyBuildResult", message: "Build running", state: "PENDING"]] ]
  ])
                    //setBuildStatus("Build running", "PENDING", "${env.GIT_URL}");
                }
            }
      }        
      stage('alpine') {
            steps{
                container(name: 'alpine'){
                    sh"""
                        sleep 60
                        printenv
                        echo hellow world
                    """
                }
            }
      }
           

    }
    post {
        success {
            setBuildStatus("Build succeeded", "SUCCESS", "${env.GIT_URL}");
        }
        failure {
            setBuildStatus("Build failed", "FAILURE", "${env.GIT_URL}");
        }
    }   
}
