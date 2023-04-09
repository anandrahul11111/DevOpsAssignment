def modules = ['all-post-service', 'edit-post-service', 'create-post-service', 'like-post-service', 'memories-ui'];
def lastSuccessfulBuild(passedBuilds, build) {
  if ((build != null) && (build.result != 'SUCCESS')) {
      passedBuilds.add(build)
      lastSuccessfulBuild(passedBuilds, build.getPreviousBuild())
   }
}

@NonCPS
def getChangeLog(passedBuilds) {
    def log = ""
    for (int x = 0; x < passedBuilds.size(); x++) {
        def currentBuild = passedBuilds[x];
        def changeLogSets = currentBuild.rawBuild.changeSets
        for (int i = 0; i < changeLogSets.size(); i++) {
            def entries = changeLogSets[i].items
            for (int j = 0; j < entries.length; j++) {
                def entry = entries[j]
                log += "${entry.commitId} \n"
            }
        }
    }
    return log;
  }


def getCommitAuthor(commitId){
    return sh(returnStdout: true, script: "git log --pretty=format:\"%ae\" ${commitId}").trim()
// 	return sh(returnStdout: true, script: "git log --pretty=format:\"%ae\"").trim()
            .split("\n")
            .collect { it.trim() }
            .unique()
            .findAll { it != 'noreply-github+ms@sap.com' }
}
def determineCommitAuthor(currentBuild) {
    def ids = []
	def authors = []
    def msgs = []
    jenkinsCustomData = [:];

    if ( currentBuild.changeSets ) {
// 	    currentBuild.changeSets.each { changeSet ->
// 	    changeSet.each { entry ->
// 		println "Commit ID: ${entry.commitId}"
// 		println "Message: ${entry.msg}"
// 		print("new author=="+getCommitAuthor(entry.commitId))
// 	    }
// 	}
	    
// 	def commitIds = []
// 	def gitLog = sh(script: 'git log --oneline', returnStdout: true).trim()
// 	def lines = gitLog.split('\n')
// 	lines.each { line ->
// 	  def commitId = line.tokenize()[0]
// 	  commitIds.add(commitId)
// 	}
// 	println commitIds
// 	print("new author=="+getCommitAuthor(commitIds[0]))

        for (def changeLog in currentBuild.changeSets) {
		print"Changelogs: $changeLog"
            def entries = changeLog.items
		print"entries: $entries"
            for (def entry in entries) {
            print("ids=="+entry.commitId.toString()+" author=="+entry.author.toString()+ " message=="+entry.msg)
                ids << entry.commitId.toString()
                authors << entry.author.toString()
                msgs << entry.msg
// 	    print"ids==$ids"
//             print("new author=="+getCommitAuthor(entry.commitId.toString()))
	    }
        }
        
        jenkinsCustomData['commit_id'] = ids.join(",")
        jenkinsCustomData['commit_author'] = authors.join(",")
        jenkinsCustomData['commit_message']= msgs.join(",")
    } else {
        print("Error: No changeset found in currentBuild")
    }

    print("Jenkins Custom Data for Change set " + jenkinsCustomData)
    return jenkinsCustomData
}
pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                script {
//                     print"current build: $currentBuild.changeSets"
// 		    author= sh(script: 'git log -1 --pretty=%"ae" ${GIT_COMMIT}', returnStdout: true).trim()
//             	    print("author: $author")
//                     jenkinsCustomData = determineCommitAuthor(currentBuild)
		passedBuilds = []
		  lastSuccessfulBuild(passedBuilds, currentBuild);
		  def changeLog = getChangeLog(passedBuilds)
		  echo "changeLog ${changeLog}"
                }
            }
        }
//          stage('Test') {
//             steps {
//                 script {
//                      modules.each { item -> 
//                         dir("${item}"){
//                         print("Testing Code for $item")
//                         bat "npm run sonar" 
//                         }
//                     }
//                 }
//             }
//         }
//         stage('Dockerizing'){
//             steps {
//                 script {
//                      modules.each { item -> 
//                         dir("${item}"){
//                         print("Dockerizing microservices for $item")
//                         bat "docker build . -t heyshraddha/$item"    
                    
//                         }
//                     }
//                 }
//             }
//         }
//         stage('Pushing Docker Images'){
//             steps {
//                 script {
//                     bat "docker login -u heyshraddha -p QAZwsx@#123"
//                      modules.each { item -> 
//                         dir("${item}"){
//                         print("Pushing Docker Images to Registery for $item")
//                         bat "docker push heyshraddha/$item  "    
                    
//                         }
//                     }
//                 }
//             }
//         }
//         stage('Deploying to Kubernetes') {
//             steps {
//                 script {
//                     bat "kubectl version"
//                      modules.each { item -> 
//                         print("Deploying Micro Services to Kubernetes $item")
//                         bat "kubectl create deployment $item-cluster --image heyshraddha/$item" 
//                         bat "kubectl scale deployment $item-cluster --replicas 2"
//                         bat "kubectl expose deployment $item-cluster --type=NodePort --port 3000"
//                     }
//                     sleep(60);
//                     bat "kubectl get services"
//                     bat "kubectl get deployment"
//                 }
//             }
//         }

    }
}
