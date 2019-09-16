@Library('shared-lib')_
pipFunc() {
    git 'http://snowdevops@dnbpuls.sndevops.net:81/scm/dnbpul/dnb-web.git'
    def mvn_version = 'Maven'
    stage('compile') {
        snDevOpsStep (stepSysId:'6480283b533733007109ddeeff7b1241')
        snDevOpsChange()
        printBuildinfo {
        	name = "Compiling..."
        }
        withEnv( ["PATH+MAVEN=${tool mvn_version}/bin"] ) {
        	sh 'mvn clean install -DskipTests'
		}
    }
    stage('test') {
        snDevOpsStep (stepSysId:'e480283b533733007109ddeeff7b1241')
        printBuildinfo {
        	name = "Testing....."
        }
        withEnv( ["PATH+MAVEN=${tool mvn_version}/bin"] ) {
        	sh 'mvn test -Dpublish'
        }
        junit '**/target/surefire-reports/*.xml'
    }
    stage('package') {
        snDevOpsStep (stepSysId:'e880283b533733007109ddeeff7b1240')
        printBuildinfo {
        	name = "Packaging...."
        }
        withEnv( ["PATH+MAVEN=${tool mvn_version}/bin"] ) {
			sh 'mvn package -Dmaven.test.skip=true --batch-mode'
        }
        deploy()
    }
    stage('docker build') {
        snDevOpsStep (stepSysId:'e080283b533733007109ddeeff7b1241')
        printBuildinfo {
        	name = "Docker build...."
        }
    }
    stage('deploy') {
        snDevOpsStep (stepSysId:'6880283b533733007109ddeeff7b1241')
        snDevOpsChange()
        printBuildinfo {
             name = "Deploying...."
        }
		checkStatus()
    }
}

