import hudson.model.*
import hudson.EnvVars
import groovy.json.JsonSlurperClassic
import groovy.json.JsonBuilder
import groovy.json.JsonOutput
import groovy.json.JsonSlurper
import java.net.URL


/* Only keep the 10 most recent builds. */
def projectProperties = [
    [$class: 'BuildDiscarderProperty',strategy: [$class: 'LogRotator', numToKeepStr: '5']],
]
def imageName = 'jenkinsciinfra/jenkinsio'

if (!env.CHANGE_ID) {
    if (env.BRANCH_NAME == null) {
        projectProperties.add(pipelineTriggers([cron('H/30 * * * *'), pollSCM('H/5 * * * *')]))
    }
}

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
			stage('Reemplazando Parametros') {
            steps {
                    script {	
                   echo 'Cambio Parametros'
				   def props = readJSON file: file+'.json'
				   echo " valor:  ${props.pipelineConfig.title}"
				   echo " valor:  ${props.pipelineConfig.configuration[0].name}"
				   echo " valor:  ${props.pipelineConfig.configuration[12].name}"
				   echo " valor:  ${props.pipelineConfig.configuration[12].value}"
				   def json = JsonOutput.toJson("")
				   echo "json ----> ${props}"
				   def jsonLiteral = ["value":"127.0.0.1","key":"pMQHost"]
                   println "JSON Literal as JSON : " + JsonOutput.prettyPrint(JsonOutput.toJson(jsonLiteral))

                   props.pipelineConfig.configuration[12].value = JsonOutput.prettyPrint(JsonOutput.toJson(jsonLiteral))
				   echo "Archivo reemplazado \n ${props}"
				   props.pipelineConfig.title = 'HolaJenkinsFile'
				   writeJSON file: file+'.json', json: props

            }
        }
		}
        stage('Parametrizando'){
            steps{
                script{
               def jsonSlurper = new JsonSlurper()
               def object = jsonSlurper.parseText('{ "myList": [4, 8, 15, 16, 23, 42] }')
               assert object instanceof Map
               assert object.myList instanceof List
               assert object.myList == [4, 8, 15, 16, 23, 42]
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
		stage('Iniciando QA') {
            steps {
                    script {	
                echo 'Iniciando Pipeline ' 
                sh 'curl -X POST --header "Content-Type:application/json" --header "X-Requested-By:SDC" --data-binary "{"pMQQueue": "test_mq_queue"}" -u "admin:admin" http://127.1.1.0:18630/rest/v1/pipeline/'+pipelineName+'/start?rev=0'
            				}
            }
        }
		
    }
}








