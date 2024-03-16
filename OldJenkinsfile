def registry = 'https://algorithmtechies.jfrog.io'
def imageName = 'algorithmtechies.jfrog.io/mypypi-pypi-local/hello-world'
def version   = '1.0.0'
pipeline {
    agent
                {
                        label 'python'
                }

        environment
                {
                        PATH = "/usr/bin:$PATH"
                }

    stages {

        stage('Build') {
        steps {
                sh 'pwd'
                                sh 'python3 launch.py'
              }
        }

        stage('Python Publish') {
                        steps {
                                script {
                    echo '<--------------- Python Publish Started --------------->'
                     def server = Artifactory.newServer url:registry+"/artifactory" ,  credentialsId:"jfrog-creds"
                     def properties = "buildid=${env.BUILD_ID},commitid=${GIT_COMMIT}";
                     def uploadSpec = """{
                          "files": [
                            {
                              "pattern": "launch.py",
                              "target": "mypypi-pypi-local/{1}",
                              "flat": "false",
                              "props" : "${properties}",
                              "exclusions": [ "*.sha1", "*.md5"]
                            }
                         ]
                     }"""
                     def buildInfo = server.upload(uploadSpec)
                     buildInfo.env.collect()
                     server.publishBuildInfo(buildInfo)
                     echo '<--------------- Python Publish Ended --------------->'

            }
        }
      }
	  stage(" Docker Build ") {
		steps {
			script {
				echo '<--------------- Docker Build Started --------------->'
				app = docker.build(imageName+":"+version)
				echo '<--------------- Docker Build Ends --------------->'
			}
		}
    }

    stage (" Docker Publish "){
        steps {
            script {
               echo '<--------------- Docker Publish Started --------------->'  
                docker.withRegistry(registry, 'jfrog-creds'){
                    app.push()
                }    
               echo '<--------------- Docker Publish Ended --------------->'  
            }
        }
    }
  }
}
