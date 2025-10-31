pipeline {
    agent any

    environment {
        NODE_ENV = 'production'
    }

    stages {
        stage('Declarative: Checkout SCM') {
            steps {
                checkout scm
            }
        }

        stage('Clean Workspace') {
            steps {
                cleanWs()
            }
        }

        stage('Checkout') {
            steps {
                checkout([
                    $class: 'GitSCM',
                    branches: [[name: '*/main']],
                    doGenerateSubmoduleConfigurations: false,
                    extensions: [],
                    userRemoteConfigs: [[url: 'https://github.com/percentkimkr-prog/Jenkins_declarative_pipeline02.git']]
                ])
            }
        }

        stage('Install') {
            steps {
                bat """
                echo ====== Install 단계 시작 ======
                if exist package.json (
                    echo npm install 실행 중...
                    npm ci || npm install
                ) else (
                    echo package.json 파일이 없습니다. Install 단계 생략.
                )
                """
            }
        }

        stage('Build') {
            steps {
                bat """
                echo ====== Build 단계 시작 ======
                if exist package.json (
                    echo package.json 파일 감지됨. npm run build 실행 시도...
                    set "CI=" && npm run build || echo 빌드 명령이 정의되어 있지 않음.
                ) else (
                    echo package.json 파일 없음. Build 단계 생략.
                )
                """
            }
        }

        stage('Test') {
            steps {
                bat """
                echo ====== Test 단계 시작 ======

                REM src 폴더로 이동
                cd src

                REM hello.txt 존재 여부 확인
                if not exist hello.txt (
                    echo 실패: src\\hello.txt 파일이 존재하지 않습니다.
                    exit /b 1
                )

                REM hello.txt 안에 'hello' 문자열이 있는지 확인
                findstr /i /c:"hello" hello.txt >nul
                if errorlevel 1 (
                    echo 실패: src\\hello.txt에 'hello' 문자가 없습니다.
                    exit /b 1
                )

                echo ✅ Test 단계 성공
                """
            }
        }

        stage('Run') {
            when {
                expression { currentBuild.resultIsBetterOrEqualTo('SUCCESS') }
            }
            steps {
                echo "Run 단계 수행"
            }
        }
    }

    post {
        always {
            echo "🧾 Pipeline 종료 — Jenkins Console Output에서 세부 로그 확인 가능."
        }
        success {
            echo "✅ Pipeline 성공!"
        }
        failure {
            echo "❌ Pipeline 실패! 콘솔 로그를 확인하세요."
        }
    }
}
