def modules = ['all-post-service', 'edit-post-service', 'create-post-service', 'like-post-service', 'memories-ui'];
def getCommitAuthor(commitId){
    return sh(returnStdout: true, script: "git log --pretty=format:\"%ae\" ${commitId[0]}").trim()
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
	    print"Changesets current build: $currentBuild.changeSets"
	    def changese=sh(script: 'git log --oneline', returnStdout: true).trim()
	    print"Changesets : $changese"
        for (def changeLog in currentBuild.changeSets) {
            def entries = changeLog.items
            for (def entry in entries) {
            print("ids=="+entry.commitId.toString()+" author=="+entry.author.toString()+ " message=="+entry.msg)
                ids << entry.commitId.toString()
                authors << entry.author.toString()
                msgs << entry.msg
	    print"ids==$ids"
// 	    def id = ids[0]
		gitList = gitInfo.getGitInfo()
                GIT_COMMIT = gitList[0]
                GIT_AUTHOR = gitList[1]
		print"GIT_COMMIT==$GIT_COMMIT"
		print"GIT_AUTHOR==$GIT_AUTHOR"
		    
            print("new author=="+getCommitAuthor(ids))
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
                    print"current build: $currentBuild.changeSets"
                    jenkinsCustomData = determineCommitAuthor(currentBuild)
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
