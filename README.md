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

