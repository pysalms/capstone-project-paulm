node{
stage('checkout'){
	git 'https://github.com/pysalms/capstone-project-paulm.git'
}
stage('maven build'){
	sh 'mvn clean package'
}
stage('containerize'){
// 	sh 'docker build -t pysalms/capstone-project:1.0 .' 
}
stage('Release'){
    withCredentials([string(credentialsId: 'dockerHubPwd', variable: 'dockerHubPwd')]) {
    // 	sh "docker login -u pysalms -p ${dockerHubPwd}" 
	   // sh 'docker push pysalms/capstone-project:1.0' 
    }
}
stage('Deploy to Test'){
ansiblePlaybook become: true, credentialsId: 'ansible-key', disableHostKeyChecking: true, installation: 'ansible', inventory: '/etc/ansible/hosts', playbook: 'configure-test-server.yml', vaultTmpPath: ''
}
stage('checkout regression test source code'){
    git branch: 'main', url: 'https://github.com/pysalms/jenkins.git'
}
stage('build test scripts'){
    sh 'mvn clean package assembly:single'
}
stage('execute selenium test script'){
    sh 'java -jar target/jenkins-0.0.1-SNAPSHOT-jar-with-dependencies.jar'
}
stage('checkout'){
	git 'https://github.com/pysalms/capstone-project-paulm.git'
}
stage('Deploy to Test'){
ansiblePlaybook become: true, credentialsId: 'ansible-key', disableHostKeyChecking: true, installation: 'ansible', inventory: '/etc/ansible/hosts', playbook: 'configure-prod-server.yml', vaultTmpPath: ''
}

}
