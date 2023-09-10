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
      stage('alpine') {
            steps{
                container(name: 'alpine'){
                    sh"""
                        sleep 60
                        echo hellow world
                    """
                }
            }
      }
    }
}
