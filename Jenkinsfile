// ✅ Jenkins Declarative Pipeline (Node.js 프로젝트용, Windows 환경, bat 명령어)

pipeline {
    agent any

    environment {
        // Windows 환경에서 Node.js, npm 경로 추가
        PATH = "C:\\Program Files\\nodejs;C:\\Users\\0124e\\AppData\\Roaming\\npm;%PATH%"
    }

    stages {

        // 🧹 0️⃣ Workspace 초기화
        stage('Clean Workspace') {
            steps {
                cleanWs()
            }
        }

        // 🗂️ 1️⃣ Git 저장소에서 소스 코드 체크아웃
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        // 📦 2️⃣ Node.js 의존성 설치
        stage('Install') {
            steps {
                bat 'npm ci'
            }
        }

        // 🧪 3️⃣ 테스트 실행
        stage('Test') {
            steps {
                bat 'npm test'
            }
        }

        // 🚀 4️⃣ 애플리케이션 실행 (main 브랜치일 때만)
        stage('Start') {
            when {
                branch 'main'
            }
            steps {
                bat 'npm start'
            }
        }
    }

    post {
        success {
            echo 'Pipeline 성공적으로 완료!'
        }
        failure {
            echo 'Pipeline 실패!'
        }
    }
}
