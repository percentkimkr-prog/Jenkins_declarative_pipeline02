pipeline {
    agent any

    environment {
        PATH = "C:\\Program Files\\nodejs;C:\\Users\\0124e\\AppData\\Roaming\\npm;%PATH%"
    }

    stages {

        stage('Clean Workspace') {
            steps {
                cleanWs()
            }
        }

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        // ✅ 2️⃣ Node.js 의존성 설치 단계 추가
        stage('Install') {
            steps {
                bat '''
                    echo ====== Install 단계 시작 ======
                    if exist package.json (
                        echo npm install 실행 중...
                        npm ci || npm install
                    ) else (
                        echo package.json 파일이 없습니다. Install 단계 생략.
                    )
                    echo ====== Install 단계 완료 ======
                '''
            }
        }

        // 🧱 3️⃣ Build 단계
        stage('Build') {
            steps {
                script {
                    bat '''
                        echo ====== Build 단계 시작 ======
                        if exist package.json (
                            echo package.json 파일 감지됨. npm build 실행 시도...
                            npm run build || echo 빌드 명령이 정의되어 있지 않음.
                        ) else (
                            echo package.json 파일 없음. 빌드 단계 생략.
                        )
                        echo ====== Build 단계 완료 ======
                    '''
                }
            }
        }

        // 🧪 4️⃣ Test 단계
        stage('Test') {
            steps {
                script {
                    bat '''
                        echo ====== Test 단계 시작 ======
                        if not exist hello.txt (
                            echo 실패: hello.txt 파일이 존재하지 않습니다.
                            exit /b 1
                        )
                        findstr /i /c:"hello" hello.txt >nul
                        if errorlevel 1 (
                            echo 실패: hello.txt에 'hello' 문자가 없습니다.
                            exit /b 1
                        )
                        echo 테스트 통과: hello.txt에 'hello' 문자가 존재합니다.
                        echo ====== Test 단계 완료 ======
                    '''
                }
            }
        }

        // 🚀 5️⃣ Run 단계
        stage('Run') {
            when {
                branch 'main'
            }
            steps {
                script {
                    bat '''
                        echo ====== Run 단계 시작 ======
                        if exist main.bat (
                            echo main.bat 실행 중...
                            call main.bat
                        ) else (
                            echo 실패: main.bat 파일이 없습니다.
                            exit /b 1
                        )
                        echo ====== Run 단계 완료 ======
                    '''
                }
            }
        }
    }

    post {
        success {
            echo '✅ Pipeline 성공적으로 완료!'
        }
        failure {
            echo '❌ Pipeline 실패! 콘솔 로그를 확인하세요.'
        }
        always {
            echo '🧾 Pipeline 종료 — Jenkins Console Output에서 세부 로그 확인 가능.'
        }
    }
}
