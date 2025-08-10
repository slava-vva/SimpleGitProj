pipeline {
    agent any

    environment {
        SOLUTION_NAME = "SimpleGitProj.sln"
        BUILD_CONFIG  = "Release"
        IIS_SITE_PATH = "C:\\_Publish\\Simple_Git_Proj"
        APP_POOL_NAME = "SimpleGitProj"
        MSBUILD_EXE   = "C:\\Program Files\\Microsoft Visual Studio\\2022\\Community\\MSBuild\\Current\\Bin\\MSBuild.exe"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/slava-vva/SimpleGitProj.git'
            }
        }

        stage('Build') {
            steps {
                bat "\"${MSBUILD_EXE}\" ${SOLUTION_NAME} /p:Configuration=${BUILD_CONFIG} /t:Rebuild"
            }
        }

        stage('Deploy to IIS') {
            steps {
                bat '''
                echo Stopping App Pool
                %windir%\\System32\\inetsrv\\appcmd stop apppool /apppool.name:"${APP_POOL_NAME}"

                echo Deploying files
                xcopy /Y /E /I "$(WORKSPACE)\\SimpleGitProj\\bin\\${BUILD_CONFIG}\\*" "${IIS_SITE_PATH}\\bin\\"
                xcopy /Y /E /I "$(WORKSPACE)\\SimpleGitProj\\Views" "${IIS_SITE_PATH}\\Views"
                xcopy /Y /E /I "$(WORKSPACE)\\SimpleGitProj\\Content" "${IIS_SITE_PATH}\\Content"
                xcopy /Y /E /I "$(WORKSPACE)\\SimpleGitProj\\Scripts" "${IIS_SITE_PATH}\\Scripts"
                copy /Y "$(WORKSPACE)\\SimpleGitProj\\Web.config" "${IIS_SITE_PATH}\\Web.config"

                echo Starting App Pool
                %windir%\\System32\\inetsrv\\appcmd start apppool /apppool.name:"${APP_POOL_NAME}"
                '''
            }
        }
    }
}
