
@Library('jenkins_library@master')_
pipeline {
    agent {label "master"}
    parameters {
        string (
                description: 'component name for docker image',
                defaultValue: 'eitan-nodejs',
                name : 'component_name')
        string (
                description: 'registy url',
                defaultValue: '127.0.0.1:5000',
                name : 'registry_server')
        string (
                description: 'response when checking keep alive',
                defaultValue: 'OK',
                name : 'check_response')
    }
    stages {
        stage('Calc image name'){
            steps {
               script {
                image_name = "${params.registry_server}/${params.component_name}:${BUILD_NUMBER}"
                env.image_name = image_name
            	}
            }            
        }
        stage('Build dokcer image') {
            steps {
                script{
                docker.withTool("default") {
                    sh '''
                    	docker build -t ${image_name} .                    
                    '''                    
                    }                    
                }
            }
        }
        stage('Test container') {
            steps {
                script{
                docker.withTool("default") {
                    test_result = "Failed"
                    def my_container = docker.image("${image_name}").run("-p 80:80 --name ${params.component_name}-${BUILD_NUMBER}_test")
                    
                    result = false
                    if (my_container) {
                    	   result =  check_http("http://172.17.0.1:80/test", "${params.check_response}")
        	           if (result){
                    	      print "Test Passed"
                          }else{
                    	      print "Test Failed, Failing job!!!"
                    	      currentBuild.result = 'FAILURE'
                          }
                    	   my_container.stop()
                    	}
                    }
                }
            }
        }
        stage('Deploy') {
            when {
                expression { return result }
               }
            steps {
                echo 'Deploying....'
                script {
                  docker.withTool("default") {
                  sh '''
                     docker push ${image_name}
                  '''
                  }
                }
            }
        }
    }
    post {
        always {
            script {
		docker.withTool("default") {
                    sh '''
                    	docker rmi ${image_name}
                    '''
                   
            }
        }
    }
    }
}

