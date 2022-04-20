Timeout for stage.

```
pipeline {
    agent any

    options {
        timeout(time: 1, unit: 'HOURS')   // timeout on whole pipeline job
    }

    stages {
        stage('Example') {
          options {
              timeout(time: 1, unit: 'HOURS')   // timeout on this stage
          }
          steps {
              echo 'Hello World'
          }
        }
    }
}
```
