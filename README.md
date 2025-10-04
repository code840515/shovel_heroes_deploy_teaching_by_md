# Shovel Heroes 專案建置教學
教學導師: Discord - lian

---

## 1. 下載 Git、NodeJS、Docker

- Git  
  Download: [https://git-scm.com/downloads](https://git-scm.com/downloads)  
  ![alt text](assets/download_node_js.png)

- NodeJS  
  Download: [https://nodejs.org/zh-tw/download](https://nodejs.org/zh-tw/download)  
  ![alt text](assets/download_node_js.png)

- Docker  
  Download: [https://www.docker.com/](https://www.docker.com/)  
  注意：下載 Docker，如果是 Windows 系統，且非高通 CPU，請選擇 AMD64 版本  
  ![alt text](assets/download_node_js.png)

---

## 2. Fork 專案

打開瀏覽器，前往 [https://github.com/shovel-heroes-org/shovel-heroes](https://github.com/shovel-heroes-org/shovel-heroes) 進行 Fork。

- Fork 頁面  
  ![alt text](assets/github_fork.png)

- Fork 完成後，系統會將專案複製到你的 GitHub 資源庫底下  
  ![alt text](assets/github_fork_end.png)

---

## 3. Clone 專案到本地端

在終端機執行以下步驟，將 Fork 的專案下載到電腦桌面。

1. 取得 Git Clone 位址  
   ![alt text](assets/git_clone_address.png)

2. 進入桌面資料夾  
   ![alt text](assets/cmd_cd_desktop.png)

3. 執行 Git Clone  
   ![alt text](assets/cmd_git_clone.png)

---

## 4. 設定環境變數

Clone 完專案後，桌面會出現專案資料夾。請進入該資料夾設定環境變數

- .env檔案內容
   ```env
   DATABASE_URL=postgres://postgres:postgres@localhost:5432/shovelheroes
   PORT=8787

   # === Public App / API Base ===
   PUBLIC_APP_URL=http://localhost:5173
   PUBLIC_BASE_URL=http://localhost:8787

   # === LINE Login (OAuth 2.1 / OpenID Connect) ===
   LINE_CHANNEL_ID=
   LINE_CHANNEL_SECRET=
   LINE_REDIRECT_URI=http://localhost:8787/auth/line/callback

   # === Auth (JWT) ===
   AUTH_JWT_SECRET=dev-jwt-secret-change-me

   # === Cookies / Sessions ===
   COOKIE_SECRET=dev-cookie-secret-change-me
   COOKIE_SECURE=0

   # === Frontend Build (Vite) ===
   VITE_API_BASE=http://localhost:8787
   VITE_TURNSTILE_SITE_KEY=

   # === Bot / Abuse Prevention ===
   TURNSTILE_SECRET_KEY=
   ```

1. 在專案資料夾的根目錄底下新增 `.env` 檔案，並貼上內容：
   ![alt text](assets/setting_env1.png)

2. 在專案資料夾的packages/backend目錄底下新增 `.env` 檔案，並貼上內容：
   ![alt text](assets/setting_env2.png)

---

## 5. 啟動專案

依照專案中的 `README.md` 內「快速開始」步驟操作。

1. 在專案資料夾底下開啟終端機 (cmd)

2. 執行以下指令安裝套件：
   ```bash
   npm install
   ```
   ![alt text](assets/cmd_npm_install.png)  
   
   安裝完成後會看到 `node_modules` 資料夾  
   ![alt text](assets/file_node_modules.png)

3. 打開桌面上的 Docker Desktop，接著在cmd執行以下指令：
   ```bash
   docker-compose up -d
   ```

   執行完後即可關閉cmd  
   ![alt text](assets/cmd_docker-compose_up_-d.png)

4. 打開新的終端機，啟動後端：
   ```bash
   npm run dev:api
   ```

   後端 Port 為 8787  
   ![alt text](assets/cmd_npm_run_dev-api.png)

5. 再開一個新的終端機，啟動前端：
   ```bash
   npm run dev
   ```
   
   前端 Port 為 5173  
   此時會有兩個 cmd 視窗在運行  
   屆時造訪 [http://localhost:5173](http://localhost:5173) 即可查看前端畫面
   ![alt text](assets/cmd_npm_run_dev.png)

---

## 6. 啟動 pgAdmin

1. 打開 cmd，輸入以下指令啟動本地端的 pgAdmin：
    ```bash
    docker run --hostname=7091b8ed2551 --user=5050 \
    --env=PGADMIN_DEFAULT_EMAIL=admin@admin.com \
    --env=PGADMIN_DEFAULT_PASSWORD=admin \
    --volume=/var/lib/pgadmin \
    --network=bridge \
    --workdir=/pgadmin4 \
    -p 5051:80 \
    --restart=unless-stopped \
    --runtime=runc \
    -d dpage/pgadmin4
    ```

    ![alt text](assets/cmd_docker_pgadmin.png)

2. 執行後打開 Docker Desktop，會看到一個新的 pgAdmin 虛擬環境：

    ![alt text](assets/docker_pgadmin_virual.png)

3. 啟動後可透過對應的 Port 連線瀏覽器查看 GUI 介面：
- 登入帳號：`admin@admin.com`
- 密碼：`admin`

    ![alt text](assets/docker_pgadmin_run.png)


    ![alt text](assets/localhost_pgadmin.png)

---

## 7. 匯入 SQL 測試資料

1. 請先將 `public_test_data.sql` 放入你的"下載"資料夾中。
- 使用者名稱位置  
    ![alt text](assets/user_name_where.png)

- 資料庫容器 ID  
    ![alt text](assets/docker_postgres_view.png)
    
2. 執行以下指令匯入資料(記得更改「你的使用者名稱」及「你的資料庫容器ID」)：
    ```bash
    docker cp C:/Users/你的使用者名稱/Downloads/public_test_data.sql 你的資料庫容器ID:/tmp/public_test_data.sql; \
    docker exec -i 你的資料庫容器ID sh -c "pg_restore -U postgres -d shovelheroes --clean --if-exists --no-owner --no-privileges /tmp/public_test_data.sql"
    ```

    ![alt text](assets/cmd_docker_sql_input.png)

---

## 8. 連線 pgAdmin

1. 右鍵建立 Server Group 按「Create」→「Server Group」  
   ![alt text](assets/pgadmin_create_server_group.png)

2. 命名為 `shovel_heroes`  
   ![alt text](assets/pgadmin_create_server_group_input_name.png)

3. 點選新創建的 group，右鍵按「Register」→「Server」  
   ![alt text](assets/pgadmin_register_server.png)

4. General 頁籤 → Name 輸入 `shovel`  
   ![alt text](assets/pgadmin_register_server_input_general.png)

5. Connection 頁籤：
- Host：`host.docker.internal`
- Port：5432（預設）
- Username：`postgres`
- Password：`postgres`
- Maintenance：`shovelheroes`  

   ![alt text](assets/pgadmin_register_server_input_connection.png)

    連線成功後，可以看到「`shovel_heroes`」→「`shovel` 」 下的的兩個資料庫，其中 `shovelheroes` 為主要資料庫：

    ![alt text](assets/pgadmin_register_end.png)

    完成後，請將前後端關閉並重新啟動，造訪 [http://localhost:5173](http://localhost:5173)即可看到後端撈取的 SQL 資料。

---

## 9. 註冊 LINE BOT

註冊連結：[https://developers.line.biz/en/docs/messaging-api/using-bot-designer/](https://developers.line.biz/en/docs/messaging-api/using-bot-designer/)

1. 點選 Login in to Console  
   ![alt text](assets/line_bot_step1.png)

2. 使用 LINE 帳號登入  
   ![alt text](assets/line_bot_step2.png)
   
   ![alt text](assets/line_bot_step3.png)

3. 建立 Provider  
   ![alt text](assets/line_bot_step4.png)

   ![alt text](assets/line_bot_step5.png)

4. 建立 LINE Login Channel  
   ![alt text](assets/line_bot_step6.png)

   ![alt text](assets/line_bot_step7.png)

   ![alt text](assets/line_bot_step8.png)
   
   ![alt text](assets/line_bot_step9.png)

5. 取得 Channel 參數，並填入 `.env`：
- Channel ID  
    ![alt text](assets/line_bot_step10.png)

- Channel Secret  
    ![alt text](assets/line_bot_step11.png)

- Channel Callback URL  
    ![alt text](assets/line_bot_step12.png)

- 編輯專案根目錄及packages/backend目錄中 `.env` 的 LINE 相關參數  
    ![alt text](assets/setting_env_line.png)

    完成後，請將前後端關閉並重新啟動，造訪 [http://localhost:5173](http://localhost:5173)即可使用登入功能。