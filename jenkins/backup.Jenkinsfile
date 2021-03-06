@Library('pipeline-library-demo')_

pipeline {
    agent any
    

    environment {
      Integer deployed_to_qa = 91
      Integer ready_for_po = 6
      jiraSite = "Hertz-Jira"  // Hertz site
      ddtl_ID =  "ddtl-1122"
      jiraComment = "No Comment"
      gitCommitMsg = ""
      current_state = ""

      //initializing a local variable
      in_progress = "https://hertzdigital.atlassian.net/rest/api/2/status/11408"
      dev_complete = "https://hertzdigital.atlassian.net/rest/api/2/status/11419"
      ready_to_deploy = "https://hertzdigital.atlassian.net/rest/api/2/status/6"
      to_do = "https://hertzdigital.atlassian.net/rest/api/2/status/11300"

    }  // End of Environment block

    stages {
        stage ('check Jira') {
            steps {
                script {
                  checkJenkinsEnv 
                  checkJiraIssue
             }} // End of script and step
        } // End of check Jira stage

        stage ('Validate Jira') {
          steps {
            script {
              
              ddtl_ID = ""
              tmp = ""
              def myresult = checkJiraIssue 
            
              echo "####" + ddtl_ID + "####"
              String new_ddtl_ID = ddtl_ID

               // validate that ddtl number is found within Git gitCommitMsg message
               if (ddtl_ID == "") {

                 echo "##########################"
                 echo "\n ${BUILD_URL}"
                 echo "##########################"

                 currentBuild.result = 'FAILURE'


               } else {
                 echo "Following DDTL IDs retrieved from git commit message"
                 echo ddtl_ID
               }

                 // retrieving jira issue
              try {
               /*
                echo "Executing jiraGetFields"
                def fields = jiraGetFields idOrKey: ddtl_ID, site: jiraSite
                echo fields.data.toString()
                echo "Executing jiraGetProjectStatuses"
                def statuses = jiraGetProjectStatuses idOrKey: ddtl_ID, site: jiraSite
                echo statuses.data.toString()
              */
                 echo "Executing jiraGetIssue"
                 issue = jiraGetIssue idOrKey: new_ddtl_ID, site: jiraSite
                 current_state = issue.data.toString()

                   if ( current_state == "" ) {

                         echo "jiraGetIssue Failed to return a value"
                         echo error_msg

                         currentBuild.result = 'FAILURE'

                   } else {
                     echo "jiraGetIssue Returned successfully"
                     echo "searching for workflow state"

                     tmp = issue.data.toString()

                     sh(returnStdout: true, script: 'echo ${current_state} ')


                     sh """

                       echo "################################"
                       echo "Searching for jira issue"
                       echo ${dev_complete}
                       echo "################################"

                       echo ${tmp} | grep -i -o ${dev_complete}
                     """
                   }

              } catch ( error_issue ){
                   error_msg = "[FAILURE - 130] Failed to build \n"
                   echo ddtl_ID
                   echo error_msg
                   echo "##########################"
                   comment = "${BUILD_URL} FAILED"
                   currentBuild.result = 'FAILURE'
                   sh "exit ${error_no}"

              }  // End of Try block
            }  // End of script block
        }} // End of Validate Jira stage and Step



        stage('Build') {
            steps {
                sh 'mvn -v'
                echo "Build package"

                // if Build success then



        }} //End of Build Stage

        stage('Test') {
            steps {
                sh 'mvn test'
        }} // End of Test Stage
        stage('Deploy to Dev') {
            steps {
                echo "Deploy to Dev"
                sh 'ls -l'
                script {
                  def error_msg
                  jiraCommet = "Jenkins Automation\n" + "\nBuild Number " + env.BUILD_NUMBER + "\nBuild URL " + env.BUILD_URL + "\nJOB NAME " + env.JOB_NAME + "\nJOB URL " + env.JOB_URL

                  jiraComment = jiraComment + "Deployment Successful - Transitioning Jira Issue to  " + gitCommitMsg + "new State " + ready_to_deploy

                  jiraAddComment idOrKey: ddtl_ID , site: jiraSite, comment: jiraComment

                    Integer pull_request_merged = 101
                    echo "##############################################################################"
                    def transitionInput = [ transition: [ id: pull_request_merged ] ]
                    try {
                      def trans_issue = jiraTransitionIssue idOrKey: ddtl_ID , input: transitionInput, site: jiraSite
                      echo "transiton response " + trans_issue.data.toString()

                    } catch (err) {
                       error "Exception"
                       jiraAddComment idOrKey: ddtl_ID , site: jiraSite, comment: "${BUILD_URL} ERROR WHILE RELEASING ${error}"
                      currentBuild.result = 'FAILURE'
                    }
                }
        }} // End of Stage and Step

        stage('Transition to QA') {
           steps {
             echo "Deploy to QA"
             script {
               def qa_transition = jiraGetIssueTransitions idOrKey: ddtl_ID, site: jiraSite
               echo qa_transition.data.toString()

               jiraCommet = "Jenkins Automation\n" + "\nBuild Number " + env.BUILD_NUMBER + "\nBuild URL " + env.BUILD_URL + "\nJOB NAME " + env.JOB_NAME + "\nJOB URL " + env.JOB_URL
               jiraComment = jiraComment + "Deployment Successful - Transitioning Jira Issue to  " + gitCommitMsg + "new State " + ready_to_deploy


                 echo "##############################################################################"
                 def transitionInput = [ transition: [ id: deployed_to_qa ] ]
                 try {
                   def trans_issue = jiraTransitionIssue idOrKey: ddtl_ID , input: transitionInput, site: jiraSite

                 } catch (err) {
                    error "Exception Failed to Transition to Jira Issue to QA"
                    jiraAddComment idOrKey: ddtl_ID , site: jiraSite, comment: "${BUILD_URL} ERROR WHILE RELEASING ${error}"
                   currentBuild.result = 'FAILURE'
                 }
             }
           }} // End Step // End Update Jira Stage

    } // End Stages
} // End Pipeline
