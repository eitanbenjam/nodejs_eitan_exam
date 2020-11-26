@Library('jenkins_library@master')_
pipeline {
    agent {label "master"}
    parameters {     
        
        string (
                description: 'gitHub repo to clone',
                defaultValue: 'https://github.com/eitanbenjam/tikal_eitan_exam.git',
                name : 'git_repo')
        string (
                description: 'component name for docker image',
                defaultValue: 'eitan-nodejs',
                name : 'component_name')
        string (
                description: 'registy url',
                defaultValue: '172.17.0.1:5000',
                name : 'registry_server')
    }
    stages {
        stage('Clone repo') {
            steps{
        	cloneRepo("${params.git_repo}", 'master', '')        
        	}
        }
        stage('Init vars'){
            steps {
               script {
                image_name = "${params.registry_server}/${params.component_name}:${BUILD_NUMBER}"
            	}
            }
            
        }
        stage('Build') {
            steps {
                script{
                docker.withTool("default") {
                    def my_container = docker.build("${image_name}", '.')
                    
                    }
                }
            }
        }
        stage('Test') {
            steps {
                script{
                docker.withTool("default") {
                    my_container = docker.image("${image_name}").run('-p 80:80')
                    result =  sh (script: 'curl http://172.17.0.1:80', returnStdout: true).trim()
                    if (result == "Hello World!"){
                    	print "Test Passed"
                    }else{
                    	print ("Test Failed")
                    }
                    my_container.stop()
                    }
                }
            }
        }
        stage('Deploy') {
            when {
                expression { return params.do_deploy }
               }
            steps {
                echo 'Deploying....'
            }
        }
    }
    post {
        always {
            script {
		summery()
            }
        }
    }
}

