// ✅ Jenkins Declarative Pipeline (Node.js 프로젝트용, Windows 환경)

pipeline {
    // 🧩 Jenkins가 어떤 에이전트(노드)에서 이 파이프라인을 실행할지 지정
    agent any

    // 🌍 파이프라인 전체에서 공통으로 사용할 환경 변수 설정
    environment {
        // ⚙️ Windows 환경에 맞게 PATH 재설정 (Node.js와 npm 경로 포함)
        PATH = "C:\\Program Files\\nodejs;C:\\Users\\0124e\\AppData\\Roaming\\npm;%PATH%"
    }

    // 🏗️ 실제 작업 단계를 정의하는 블록
    stages {

        // 🗂️ 1️⃣ Git 저장소에서 소스 코드 체크아웃
        stage('Checkout') {
            steps {
                // Jenkins가 설정된 SCM(Source Control Management)에서 자동으로 코드 가져오기
                checkout scm
            }
        }

        // 📦 2️⃣ Node.js 의존성 설치
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
                '''
            }
        }

        // 🧪 3️⃣ 테스트 실행
        stage('Test') {
            steps {
                bat '''
                    echo ====== Test 단계 시작 ======
                    if exist package.json (
                        echo npm test 실행 중...
                        npm test -- --passWithNoTests
                    ) else (
                        echo package.json 파일이 없습니다. Test 단계 생략.
                    )
                '''
            }
        }

        // 🚀 4️⃣ 애플리케이션 실행 (main 브랜치일 때만)
        stage('Start') {

            // ✅ 실행 조건 설정
            when {
                anyOf {
                    branch 'main'
                    expression { env.GIT_BRANCH == 'origin/main' }
                }
            }

            steps {
                bat '''
                    echo ====== Start 단계 시작 ======
                    if exist package.json (
                        echo npm start 실행 중...
                        npm start
                    ) else (
                        echo package.json 파일이 없습니다. Start 단계 생략.
                    )
                '''
            }
        }
    }

    // 📋 파이프라인 완료 후 실행할 후처리(post) 블록
    post {
        // ✅ 모든 단계가 성공적으로 완료되었을 때 실행
        success {
            echo '🎉 Pipeline 성공적으로 완료!'
        }

        // ❌ 하나라도 실패했을 때 실행
        failure {
            echo '❌ Pipeline 실패! 콘솔 로그를 확인하세요.'
        }

        // 📜 항상 실행되는 로그 안내
        always {
            echo '🧾 Pipeline 종료 — Jenkins Console Output에서 세부 로그 확인 가능.'
        }
    }
}
