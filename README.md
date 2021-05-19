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
