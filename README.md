📸 顔認証勤怠管理・ドアアクセス制御システム (Face Recognition Attendance System)
 
科目プロジェクト：マイクロプロセッサ工学 指導教員: Hàn Huy Dũng 博士 実施チーム: グループ01 - ハノイ工科大学 (HUST)
📖 概要 (Overview)
本プロジェクトは、低コストで実現する「スマートオフィス」ソリューションです。セキュリティ（入退室管理）と業務効率化（勤怠記録の自動化）を「2 in 1」で統合しました。
従来の指紋認証やカード認証における衛生面やカード忘れの問題を解決し、市販の顔認証機の導入コストが高いという課題を克服します。
🚀 特徴
• クライアント・サーバーアーキテクチャ: AI処理の負荷をサーバー (PC) に集中させ、ESP32 (マイコン) はデータ収集と制御に特化させることでパフォーマンスを最適化。
• 高度なアルゴリズム: InsightFace (ArcFace) を採用し、99%以上の精度を実現。斜めの顔や低照度環境でもDlib/Haar Cascadeより優れた認識率を誇ります。
• リアルタイムOS: ESP32上で FreeRTOS を採用。ビデオストリーミング、サーボ制御、OLED表示などのマルチタスクを遅延なく (< 2ms) 処理します。
• 低コスト: ハードウェア総コストは 1,000,000 VND (約6,000円) 以下。
🛠️ システムアーキテクチャ (System Architecture)
IoTクライアント・サーバーモデルに基づいています：
1. Client (Device): ESP32-CAMおよびESP32-DevKit V1を使用。画像取得、人感センサー (PIR) 検知、ドアロック制御 (Servo) を行います。
2. Server (Processing): Pythonを実行するPC。WiFi (HTTP/WebSocket) 経由で映像を受け取り、顔認証処理を行い、制御コマンドをクライアントに返します。
3. Web App: 従業員管理、カメラ映像のライブ監視、入退室履歴の確認用インターフェース。
🧰 ハードウェア (Hardware)
使用した主要コンポーネント：
コンポーネント
モデル
主な機能
メインプロセッサ
ESP32-DevKit V1
メイン制御、WiFi通信、FreeRTOS実行
カメラモジュール
ESP32-CAM (OV2640)
画像取得、ビデオストリーミング
人感センサー
PIR HC-SR501
Deep Sleepモードからのウェイクアップ
ディスプレイ
OLED 0.96" (SSD1306)
ステータス・氏名表示 (I2C通信)
モーター
Servo SG90
ドアロック機構の開閉制御
電源
Adapter 12V-2A + LM2596
安定化電源供給、電圧降下防止
💻 技術スタック (Tech Stack)
組み込み (ESP32)
• Framework: Arduino IDE / ESP-IDF
• OS: FreeRTOS (タスク管理: Camera, Servo, WiFi)
• Protocol: HTTP Request, WebSocket
コンピュータビジョン & AI (Server)
• Language: Python 3.x
• Core AI: InsightFace (ArcFace) - 顔特徴抽出 (512-D vector)
• Face Detection: SCRFD / MobileFaceNet
• Database: SQLite/MySQL (ユーザーID, 特徴量ベクトル, ログ)
• Web Interface: Streamlit (または Flask/Django)
⚙️ インストールと設定 (Installation)
1. ハードウェア
• docs/schematic フォルダ内の配線図に従って接続してください。
• 電源に関する注意: サーボモーターとESP32-CAMの電圧低下 (Brownout) を防ぐため、安定した5V外部電源を使用してください。
2. ファームウェア (ESP32)
• Arduino IDEにて ESP32 Board Manager および関連ライブラリ (Adafruit SSD1306, ESP32Servo) をインストールします。
• config.h ファイルで WiFi情報 (SSID, Password) と Server IP を設定します。
• コードを ESP32-DevKit V1 および ESP32-CAM に書き込みます。
3. サーバー (Python)
# リポジトリのクローン
git clone https://github.com/username/project-repo.git

# ライブラリのインストール
pip install -r requirements.txt
# (必須: opencv-python, insightface, onnxruntime, numpy, streamlit...)

# サーバーの起動
python main.py

# ダッシュボードの起動
streamlit run dashboard.py
👥 チームメンバー (Team Members)
グループ01 - マイクロプロセッサ工学クラス:
No.
氏名
学籍番号
担当役割
1
Nguyễn Minh Hùng
20234011
組み込み開発 (FreeRTOS, Servo, OLED)
2
Nguyễn Quốc Huy
20234013
システム設計、ドキュメント・レポート作成
3
Phan Nguyễn Mạnh Lợi
20234020
ハードウェア (回路, カメラ, 筐体)
4
Hoàng Tuấn Ngọc
20234028
AI開発 (InsightFace), サーバー & Webアプリ
📊 実験結果 (Results)
• 精度: > 96% (InsightFace)、45度以上の顔角度でも認識可能。
• 速度: 安定したLAN環境下で、ドア解錠まで 2秒未満。
• 信頼性: WatchdogタイマーとFreeRTOSにより、システムのフリーズを防止。
