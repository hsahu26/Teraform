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
        // Below step is working fully 
     stage ('test'){
        steps {
            script {
                def subfolders = sh(returnStdout: true, script: 'git diff --name-only $(git rev-parse --short HEAD^) $(git rev-parse --short HEAD) |xargs dirname| egrep "us-non-prod-epic-realtalk|us-nonprod-devops-vault|us-nonprod-devops"').trim().split(System.getProperty("line.separator"))
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
                    
                    }
                   */
                    }
                }
            }
        } 
     }
        
    }
}
