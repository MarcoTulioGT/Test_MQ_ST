import hudson.model.*
import hudson.EnvVars
import groovy.json.JsonSlurperClassic
import groovy.json.JsonBuilder
import groovy.json.JsonOutput
import groovy.json.JsonSlurper
import java.net.URL

properties(projectProperties)

pipeline {
    agent any
     environment {
    def branch = 'master'
    def pipelineName = 'TestMQ80019021-4860-40fa-82bb-296dafb1703e'
	def file= 'Test_MQ'
    def workspace = pwd()

}
    stages {
        stage('Parametrizando'){
            steps{
                script{
                        def jsonSlurper = new JsonSlurper()
                        def object = jsonSlurper.parseText('{"name":"constants","value":[{"value":"127.0.0.1", "key":"pMQHost"},{"value": "Queue_prueba","key": "pMQQueue"},{"value": "guest","key": "pMQUser"},{"value": "guest","key": "pMQPassword"}]} /* some comment */')
                        assert object instanceof Map
                        assert object.value instanceof List
                }
            }
        }
        stage('Despliegue QA') {
            steps {
                    script {	
                   echo 'Desplegando Pipeline --> ' +pipelineName
				  sh 'curl -X POST --header "Content-Type:application/json" --header "X-Requested-By:SDC" --data @'+file+'.json -u "admin:admin" http://127.1.1.0:18630/rest/v1/pipeline/'+pipelineName+'/import?rev=0&overwrite=true&autoGeneratePipelineId=false&includeLibraryDefinitions=true'
                }
            }
        }
		
    }
}








