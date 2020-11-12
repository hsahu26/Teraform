def GIT_PREVIOUS_COMMIT = "\$(git rev-parse --short HEAD^)"
def GIT_COMMIT = "\$(git rev-parse --short HEAD)"

pipeline {
    agent any
    stages {
     stage ('checkout'){
        steps {
            checkout([$class: 'GitSCM', branches: [[name: '*/main']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: 'GitHub-id', url: 'https://github.com/hsahu26/Teraform.git']]])
            
        }
    }
      
     stage ('findchanges'){
        steps {
            script {
                env.Check = sh(script: "dirname \$(git diff --name-only ${GIT_PREVIOUS_COMMIT} ${GIT_COMMIT})", returnStdout: true).trim()
                sh 'for x in $(git diff --name-only $(git rev-parse --short HEAD^) $(git rev-parse --short HEAD)  |xargs dirname); do cd $x; pwd; cd ..; done'
            }
        }
         
    }
     stage ('test'){
        steps {
            script {
                def subfolders = sh(returnStdout: true, script: 'git diff --name-only $(git rev-parse --short HEAD^) $(git rev-parse --short HEAD)  |xargs dirname').trim().split(System.getProperty("line.separator"))
                for (int i = 0; i < subfolders.size(); ++i) {
                    echo "Testing the ${subfolders[i]} folder"
                    def SEC = "${subfolders[i].toString().toUpperCase()}-SECRET"
                    echo "${SEC}"
                    dir("${subfolders[i]}"){
                //      withCredentials([string(credentialsId: "${SEC}", variable: 'var1')]) {
                    sh 'pwd'
                /*    sh 'terraform -v'
                    sh 'terraform init'
                    sh 'terraform validate'
                    sh 'terraform plan -var subscription_id=b9f49eaf-9ae1-48af-b401-97c811418ee5 -var client_id=0b60c761-567c-4acc-be60-7e6717b2e029 -var client_secret=${var1} -var tenant_id=ff3213cc-c3f6-45d4-a104-8f7823656fec'
                    }
                   */
                    }
                }
            }
        } 
     }
        
    }
}
