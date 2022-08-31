node("kubernetsagent") {
    def app
     stage('Check Dummy') {
        sh 'echo "**** Dummy work *****"'
    }
    stage('Clone repository') {
        checkout scm
    }
	/*dir('CalibrationResults') {
        git url: 'https://github.com/abhi7620/argocd-kubernets.git'
    }*/
    stage('Build image') {  
       app = docker.build("abhidockerhub7620/argocdtest")
    }

    stage('Test image') {
        app.inside {
            sh 'echo "Tests passed"'
        }
    }

    stage('Push image') {
        
        docker.withRegistry('https://registry.hub.docker.com', 'DocerkHub') {
            app.push("${env.BUILD_NUMBER}")
        }
    }
    
    stage('Trigger ManifestUpdate') {
                echo "triggering updatemanifestjob"
                build job: 'updatemanifest', parameters: [string(name: 'DOCKERTAG', value: env.BUILD_NUMBER)]
        }
}
