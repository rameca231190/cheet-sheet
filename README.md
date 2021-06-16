# Git sheet


### To set username and password credentials in github outomatically 
```
git config --global user.name "your username"

git config --global user.password "your password"
```


# Docker sheet

### Build image

```
docker  build -t  flask:v1 .
```

### Run image

```
docker run -dti -p 5000:5000 --name=flask 75dd93600ee7

-p first port is port on which it will run in the host, second port container port where app running
```


### Python flask

```
$ export FLASK_APP=hello.py
$ python -m flask run
 * Running on http://127.0.0.1:5000/
```

### Python

check exit code of os.system last command.

```
import os
import subprocess
import sys

home_dir = os.system("ls")
print("%d" % home_dir)
```



## k8s commands



```
# To check if you as a user can do something specific
k auth can-i create deployments

# For different user
k auth can-i create pods --as dev-user --namespace tesst

# Check same for service accounts
```

# Identify the authorization modes configured on the cluster

```
kubectl describe pod kube-apiserver-controlplane -n kube-system | grep -i mode

```

# To check files contain the kubelet configuration

```
ps -ef | grep /usr/bin/kubelet | grep -i config
```

# Curl the pod thru kubelet

```
curl -sk https://localhost:10250/pods
```

Check metrics on kubelet read only port

```
curl -sk http://localhost:10255/metrics 

# To disable set read only port to 0 in kubelet.conf <readOnlyPort: 0>
```

#Alias for k8s

```
alias k='kubectl'
```


# Groovy

Manual Judgement
```
   stage('Manual Judgement For DEV') {
            agent { label 'docker' }
            steps {
                timeout(time:8, unit:'HOURS') {
                    input("Proceed to DEV ?")
                }
            }
        }
```

Paremether choice
```
pipeline {
    parameters{
        choice(name: 'CLUSTER_ENV', choices: "None\sbx\ndev\nqa\nstage", description: 'Select the desire environment')
    }
```


When any of

```
stage ("Initializing SBX") {
    when {
      anyOf{
        expression { params.CLUSTER_ENV == 'sbx' }
      }
    }
```

When condition before agent
```
stage ("Initializing DEV") {
    when {
       beforeAgent true
       expression { params.CLUSTER_ENV == 'dev' }
    }

```

When conditino for branches

```
stage('master-branch-stuff') {
    when {
        branch 'master'
    }
    steps {
        echo 'run this stage - ony if the branch = master branch'
    }
}
```

```
stage('feature-branch-stuff') {
    when {
        branch 'feature/*'
    }
    steps {
        echo 'run this stage - only if the branch name started with feature/'
    }
}
```

```
stage('expression-branch') {
    when {
        expression {
            return env.BRANCH_NAME != 'master';
        }
    }
    steps {
        echo 'run this stage - when branch is not equal to master'
    }
}
```

```
stage('env-specific-stuff') {
    when { 
        environment name: 'NAME', value: 'this' 
    }
    steps {
        echo 'run this stage - only if the env name and value matches'
    }
}

```

Post actions
Always
```
post {
     always {
         script {
             sh "find . -type d -name .terraform -exec rm -rfv {} +"
         }
     }
}

```
Failed and success

```
post {
    failure {
        mail (to: 'email@gmail.com',
        subject: "Job '${env.JOB_NAME}' (${env.BUILD_NUMBER}) failed.", 
        body: "Please visit ${env.BUILD_URL} for further information."
        );
    }
    success{
        mail (to: 'email@gmail.com',
        subject: "Job '${env.JOB_NAME}' (${env.BUILD_NUMBER}) success.",
        body: "Please visit ${env.BUILD_URL} for further information."
        );
    }
}

```

