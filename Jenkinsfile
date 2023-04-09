def modules = ['all-post-service', 'edit-post-service', 'create-post-service', 'like-post-service', 'memories-ui'];
// def getCommitAuthor(commitId){
//     return sh(returnStdout: true, script: "git log -1 --pretty=format:\"%ae\" ${commitId}").trim()
//             .split("\n")
//             .collect { it.trim() }
//             .unique()
//             .findAll { it != 'noreply-github+ms@sap.com' }
// }
// def getCommitAuthor(id){
//     def authorEmail = sh returnStdout: true, script: "git log -1 --pretty=tformat:'%ae' ${id}"
//     return authorEmail;
// }
// def determineCommitAuthor(currentBuild) {
//     def ids = []
// 	def authors = []
//     def msgs = []
//     jenkinsCustomData = [:];
// 	if ( currentBuild.changeSets ) {
// 	    currentBuild.changeSets.each { changeSet ->
// 	    changeSet.each { entry ->
// 		ids << entry.commitId
// // 		authors << getCommitAuthor(ids[0])
// 		msgs << entry.msg
// 	    }
// 	}
// //         for (def changeLog in currentBuild.changeSets) {
// // 		print"Changelogs: $changeLog"
// //             def entries = changeLog.items
// // 		print"entries: $entries"
// //             for (def entry in entries) {
// //             print("ids=="+entry.commitId.toString()+" author=="+entry.author.toString()+ " message=="+entry.msg)
// //                 ids << entry.commitId.toString()
// //                 authors << entry.author.toString()
// //                 msgs << entry.msg
// // 	    print"ids==$ids"
// //             print("new author=="+getCommitAuthor(entry.commitId.toString()))
// // 	    }
// //         }
// //         print("new author=="+getCommitAuthor(logs))
//         jenkinsCustomData['commit_id'] = ids.join(",")
// //         jenkinsCustomData['commit_author'] = authors.join(",")
//         jenkinsCustomData['commit_message']= msgs.join(",")
//     } else {
//         print("Error: No changeset found in currentBuild")
//     }

// //     print("Jenkins Custom Data for Change set " + jenkinsCustomData)
//     return jenkinsCustomData
// }

def determineCommitAuthor(currentBuild) {
    jenkinsCustomData = getAuthor(currentBuild)
    def authors=[]
	for id in jenkinsCustomData['commit_id'].split(","){
		authors.add(getCommitAuthor(id))
	}
	jenkinsCustomData['commit_author'] = authors.join(",")
//     jenkinsCustomData['commit_author'] = getCommitAuthor(jenkinsCustomData['commit_id'].split(",")[0])
    return jenkinsCustomData
}

def getAuthor(currentBuild){
    def ids = []
	def authors = []
    def msgs = []
    jenkinsCustomData = [:];

    if ( currentBuild.changeSets ) {
        for (def changeLog in currentBuild.changeSets) {
            def entries = changeLog.items
            for (def entry in entries) {
            print("ids=="+entry.commitId.toString()+" author=="+entry.author.toString()+ " message=="+entry.msg)
                ids << entry.commitId.toString()
                msgs << entry.msg
            }
        }
        
        jenkinsCustomData['commit_id'] = ids.join(",")
        jenkinsCustomData['commit_message']= msgs.join(",")
    } else {
        print("Error: No changeset found in currentBuild")
    }

    print("Jenkins Custom Data for Change set " + jenkinsCustomData)
    return jenkinsCustomData
}

def getCommitAuthor(id){
    def authorEmail = sh returnStdout: true, script: "git log -1 --pretty=tformat:'%ae' ${id}"
    return authorEmail;
}

pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                script {
                    print"current build: $currentBuild.changeSets"
// 		    author= sh(script: 'git log -1 --pretty=%"ae" ${GIT_COMMIT}', returnStdout: true).trim()
//             	    print("author: $author")
                    jenkinsCustomData = determineCommitAuthor(currentBuild)
// 		    jenkinsCustomData['commit_author'] = getCommitAuthor(jenkinsCustomData['commit_id'])
		    print("Jenkins Custom Data for Change set " + jenkinsCustomData)
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
