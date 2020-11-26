@Library('jenkins_library@master')_
pipeline {
    agent {label "master"}
    parameters {     
        choice (
                choices: '1\n2',
                description: 'choose vmware system',
                name : 'my_choice')   
        booleanParam (
                defaultValue: false,
                description: 'do deploy',
                name : 'deploy')
        string (
               
                description: 'heat files zip url',
                defaultValue: '',
                name : 'my_str')
    }
    stages {
        stage('Build') {
            steps {
                say_hello("hello from lib")
                print_map(
			"a": "this is a",
			"b": "this is b"
                )
                echo 'Building..'
            }
        }
        stage('Test') {
            steps {
                cloneRepo('file:///home/vagrant/jenkins-pipeline-examples/local_git/test_project', 'master', '')
                echo 'Testing..'
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
