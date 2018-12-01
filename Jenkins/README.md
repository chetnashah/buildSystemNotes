
### Jenkins

It is JAVA based tool that requires JDK/tomcat in order to run.
Good idea to set it up on a dedicated server.

### Config file: `JenkinsFile`

The name of the config file on a project level is `JenkinsFile`.

### Pipeline

A continuous delivery (CD) pipeline is an automated expression of your process for getting software from version control right through to your users and customers. Every change to your software (committed in source control) goes through a complex process on its way to being released. This process involves building the software in a reliable and repeatable manner, as well as progressing the built software (called a "build") through multiple stages of testing and deployment.

* Two syntaxes - `Declarative Pipeline syntax` and `Scripted Pipeline syntax`.

`agent` instructs Jenkins to allocate an executor (on any available agent/node in the Jenkins environment) and workspace for the entire Pipeline.

#### Definition

A Pipeline is a user-defined model of a CD pipeline. A Pipeline’s code defines your entire build process, which typically includes stages for building an application, testing it and then delivering it.

#### Node

A node is a machine which is part of the Jenkins environment and is capable of executing a Pipeline.

#### Stage

A stage block defines a conceptually distinct subset of tasks performed through the entire Pipeline (e.g. "Build", "Test" and "Deploy" stages), which is used by many plugins to visualize or present Jenkins Pipeline status/progress.

#### Step

A single task. Fundamentally, a step tells Jenkins what to do at a particular point in time (or "step" in the process). For example, to execute the shell command make use the sh step: sh 'make'. When a plugin extends the Pipeline DSL, [1] that typically means the plugin has implemented a new step.

#### Declarative Pipeline syntax

```
Jenkinsfile (Declarative Pipeline)
pipeline { 
    agent any 
    stages {
        stage('Build') { 
            steps { 
                sh 'make' 
            }
        }
        stage('Test'){
            steps {
                sh 'make check'
                junit 'reports/**/*.xml' 
            }
        }
        stage('Deploy') {
            steps {
                sh 'make publish'
            }
        }
    }
}
```

#### Scripted pipeline syntax

```
Jenkinsfile (Scripted Pipeline)
node { 
    stage('Build') { 
        sh 'make' 
    }
    stage('Test') {
        sh 'make check'
        junit 'reports/**/*.xml' 
    }
    stage('Deploy') {
        sh 'make publish'
    }
}
```