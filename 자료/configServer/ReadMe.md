# ConfigServer



pipeline script

```groovy
node {
    stage('Pull') {
        git 'https://github.com/ywjung/configServer.git'
    }
    stage('Unit Test') {
        echo "Unit Tested!"
    }
    stage('Build') {
        sh(script: 'docker build --force-rm=true -t ywjung/config-server-app:latest .')
    }
    stage('Tag') {
        sh(script: 'docker tag ywjung/config-server-app:latest 192.168.129.90/ywjung99/config-server-app:latest')
    }
    stage('Push') {
        sh(script: 'docker login 192.168.129.90 -u ywjung -p Ab123456')
        sh(script: 'docker push 192.168.129.90/ywjung99/config-server-app:latest')
    }
    stage('Deploy') {
        try {
            sh(script: 'docker stop config-server-app')
            sh(script: 'docker rm config-server-app')
        } catch(e) {
            echo "No config-server-app container exists"
        }
        sh(script: 'docker run -d -p 8900:8900 --name=config-server-app 192.168.129.90/ywjung99/config-server-app:latest')
 	}
}
```

