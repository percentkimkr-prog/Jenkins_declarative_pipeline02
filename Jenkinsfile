// ✅ Jenkins Declarative Pipeline (Windows 환경, Groovy 실습 요구사항 반영)

pipeline {
    agent any

    // 🌍 사용자 환경 변수 설정 (Windows용)
    environment {
        PATH = "C:\\Program Files\\nodejs;C:\\Users\\0124e\\AppData\\Roaming\\npm;%PATH%"
    }

    stages {

        // 🧹 0️⃣ 워크스페이스 초기화
        stage('Clean Workspace') {
            steps {
                cleanWs()
            }
        }

        // 🗂️ 1️⃣ Git 저장소 체크아웃
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        // 🧱 2️⃣ Build 단계 (필요 시 빌드 명령 추가)
        stage('Build') {
            steps {
                script {
                    bat '''
                        echo ====== Build 단계 시작 ======
                        rem 예시: npm build 또는 다른 빌드 명령
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

        // 🧪 3️⃣ Test 단계 (hello.txt 검증)
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

        // 🚀 4️⃣ Run / Deploy 단계 (main 브랜치일 때만)
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

    // 📋 파이프라인 완료 후 처리
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

