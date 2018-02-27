# Platform.sh CLI
An alpine image with the Platform.sh CLI installed.  

## Usage

```bash
docker run -it -e PLATFORMSH_CLI_TOKEN millerrs/platformsh-cli
```

## Usage in Jenkins Pipeline

1. Install [Jenkins SSH Agent Plugin](https://wiki.jenkins.io/display/JENKINS/SSH+Agent+Plugin)
2. Add your ssh key and Platform.sh token in the global credentials section of Jenkins [(instructions)](https://support.cloudbees.com/hc/en-us/articles/203802500-Injecting-Secrets-into-Jenkins-Build-Jobs)
3. Update the credential and project IDs in the pipeline

```groovy
stage('Platform.sh') {
    environment {
        PLATFORMSH_CLI_TOKEN = credentials("platform-cli-token")
        PLATFORMSH_PROJECT_ID = 'platform-project-id'
    }
    agent {
        docker {
            image 'millerrs/platformsh-cli'
        }
    }
    steps {
        sshagent(['jenkins-ssh-platform']) {
            sh './run-your-script.sh'
        }
    }
}
```