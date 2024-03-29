# Git sheet

## Linux

Awk from the one word

Everything what goes after -F (m-) is the symbol after which it should grep
```
| awk -Fm- '{ print $2 }'
```

Telnet with curl

```
curl -v telnet://127.0.0.1:22
```

Delete failed pods in all namespaces
```
kubectl get pods -A| grep -v "Running" | grep -v "Completed" | awk '{system("kubectl -n " $1 " delete po " $2 " --force --grace-period=0")}'
```


## Inside alpine container


Install curul and telnet
```
apk update
apk add curl jq
apk add tcptraceroute
```



## Helm template

Configmap from the file dynamic name
```
{{ printf "%s.krb5.conf" .Values.KRB5_REALM | .Files.Get | indent 4 }}
Or you could parenthesize the expression:

{{ .Files.Get (printf "%s.krb5.conf" .Values.KRB5_REALM) | indent 4 }}

```

## Terraform

Import tfstate resource GCP

```
Import
TargetInstance can be imported using any of these accepted formats:

$ terraform import google_compute_target_instance.default projects/{{project}}/zones/{{zone}}/targetInstances/{{name}}
$ terraform import google_compute_target_instance.default {{project}}/{{zone}}/{{name}}
$ terraform import google_compute_target_instance.default {{zone}}/{{name}}
$ terraform import google_compute_target_instance.default {{name}

```

Install terraform 13.5.5

```
wget https://releases.hashicorp.com/terraform/0.13.5/terraform_0.13.5_linux_amd64.zip
sudo mv terraform /usr/local/bin/
```

Terraform switch versions

```
tfenv list-remote
tfenv install 0.11.14
tfenv use 0.12.0
```


### To set username and password credentials in github outomatically 
```
git config --global user.name "your username"

git config --global user.password "your password"
```


# Systemctl

## To list all the services
```
systemctl list-units --type service
```
### To disable and remove unwanted services
```
systemctl stop apache
systemctl disable apache
apt remove apache
```

## Find out systemctl service
```
netstat -tulpn | grep 8088
systemctl list-units --type service | grep -i openlitespeed
systemctl stop lshttpd.service
systemctl disable lshttpd.service
apt list --installed | grep openlitespeed
apt remove openlitespeed
```
## Remove the unit file 
```
rm /lib/systemd/system/nginx.service
```

## List all installed packages
```
apt list --installed
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

Linux command with return code

```
from subprocess import run

print( 'Running command...' )
p = run( [ 'cat', '/foo' ] )
if p.returncode != 0:
    raise Exception( f'Invalid result failed: { p.returncode }' )
    

print( 'All done!' )

```



## k8s commands



```
# To check if you as a user can do something specific
k auth can-i create deployments

# For different user
k auth can-i create pods --as dev-user --namespace tesst

# Check same for service accounts
```

#Since the kube-apiserver is running as pod you can check the process to see enabled and disabled plugins.


```
ps -ef | grep kube-apiserver | grep admission-plugins
```

#Check what is the usage for containers to different pods

```
 k top po -n test --containers
```

# Identify the authorization modes configured on the cluster

```
kubectl describe pod kube-apiserver-controlplane -n kube-system | grep -i mode

```

# To check files contain the kubelet configuration

```
ps -ef | grep /usr/bin/kubelet | grep -i config
```

# Curl endpoint with response on seconds

```
curl -o /dev/null -s -w %{time_total}  endpoint:port/path
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
# Ubuntu

Check distribution version
```
lsb_release -a
cat /etc/lsb-release
```
Upgrade to Ubuntu 20.04 LTS (Focal Fossa)
```
sudo apt install update-manager-core
sudo apt-get update
sudo apt-get upgrade -y
sudo apt-get dist-upgrade -y
```
#After performing the above actions, we can proceed to the next step.

```
sudo do-release-upgrade
But this time usually reminds you that you need to reboot, so let's reboot.

promt 'yes' everywhere


sudo reboot
```

# Istio


Use istioctl to check proxy server config.

```
istioctl proxy-config clusters <<name-of-pod>> -n <<namespace-where-pod-is-deployed>>
```

Configure istio proxy to make discovery not to all cluster services but only to the ones it needs with istio.
```
apiVersion: networking.istio.io/v1alpha3
kind: Sidecar
metadata:
  name: default
  namespace: istio-system
spec:
  egress:
  - hosts:
    - "./*"
    - "istio-system/*"
```

If using probes and istio-proxy as a sidecar add bellow annotation to pod template to avoid connection issues.

```
annotations:
  sidecar.istio.io/rewriteAppHTTPProbers: "true"
```

# Groovy Jenkins

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

Clone repo inside docker container

```
checkout([$class: 'GitSCM', branches: [[name: '*/main']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: 'cred_id', url: 'repo_url']]])
```

Paremether choice
```
pipeline {
    parameters{
        choice(name: 'CLUSTER_ENV', choices: "None\sbx\ndev\nqa\nstage", description: 'Select the desire environment')
        string(
          name: 'INPUT_ID', 
          defaultValue: 'None', 
          description: 'Enter the Input ID (Between 9000 - 9999)'
          )
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

Post clean ws
```
post { 
        always { 
            cleanWs()
        }
    }
```


Trigger Jenkins pipeline thru CLI

```
CLUSTER_ENV="sbx"
username="username"
token="TOKEN"

 
curl -X POST -I -v --insecure -u $username:$token "https://jenkins.endpoint.com/job/Folder_name/job/job-name/job/develop/buildWithParameters?token=my-token&CLUSTER_ENV=$CLUSTER_ENV"
```

Branch name env jenkins

```
environment {
        BRANCH_NAME = "${GIT_BRANCH.split('/').size() > 1 ? GIT_BRANCH.split('/')[1..-1].join('/') : GIT_BRANCH}"
        IMAGE_TAG = "${BRANCH_NAME}_${env.BUILD_ID}"
        registry = 'https://registry.hub.docker.com'
        repository = 'example/base-image'
    }
```

# Istio envoy disable

```
apiVersion: networking.istio.io/v1beta1
kind: DestinationRule
metadata:
  annotations:
  name: kiali
  namespace: istio-system
spec:
  host: kiali.istio-system.svc.cluster.local
  trafficPolicy:
    tls:
      mode: DISABLE
```

```
apiVersion: networking.istio.io/v1beta1
kind: VirtualService
metadata:
  annotations:
  name: kiali-virtual-service
  namespace: istio-system
spec:
  gateways:
  - istio-system/https-gateway
  hosts:
  - example.com
  http:
  - match:
    - uri:
        prefix: /kiali/
    route:
    - destination:
        host: kiali
        port:
          number: 20001
```

