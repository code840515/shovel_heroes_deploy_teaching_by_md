# Shovel Heroes 專案建置教學

**指導老師**：Discord - lian

---

# 測試用資料庫檔案

先行下載專案底下的`public_test_data.sql`，以便後續步驟使用

[下載 public_test_data.sql](./public_test_data.sql)

## 1. 下載 Git、NodeJS、Docker

- Git  
  Download: [https://git-scm.com/downloads](https://git-scm.com/downloads)  
  ![alt text](assets/01-01-download_git.png)

- NodeJS  
  Download: [https://nodejs.org/zh-tw/download](https://nodejs.org/zh-tw/download)  
  ![alt text](assets/01-02-download-nodejs.png)

- Docker  
  Download: [https://www.docker.com/](https://www.docker.com/)  
  注意：下載 Docker，如果是 Windows 系統，且非高通 CPU，請選擇 AMD64 版本  
  ![alt text](assets/01-03-download-docker.png)

---

## 2. Fork 專案

打開瀏覽器，前往 [https://github.com/shovel-heroes-org/shovel-heroes](https://github.com/shovel-heroes-org/shovel-heroes) 進行 Fork。

- Fork 頁面  
  ![alt text](assets/02-01-github-fork.png)

- Fork 完成後，系統會將專案複製到你的 GitHub 資源庫底下  
  ![alt text](assets/02-02-github-fork-end.png)

---

## 3. Clone 專案到本地端

在終端機執行以下步驟，將 Fork 的專案下載到電腦桌面。

1. 取得 Git Clone 位址  
   ![alt text](assets/03-01-git-clone-address.png)

2. 進入桌面資料夾  
   ![alt text](assets/03-02-cmd-cd-desktop.png)

3. 執行 Git Clone  
   ![alt text](assets/03-03-cmd-git-clone.png)

---

## 4. 設定環境變數

Clone 完專案後，桌面會出現專案資料夾。請進入該資料夾設定環境變數

- .env 檔案內容

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
   ![alt text](assets/04-01-setting-env1.png)

2. 在專案資料夾的 packages/backend 目錄底下新增 `.env` 檔案，並貼上內容：
   ![alt text](assets/04-02-setting-env2.png)

---

## 5. 啟動專案

依照專案中的 `README.md` 內「快速開始」步驟操作。

1. 在專案資料夾底下開啟終端機 (cmd)

2. 執行以下指令安裝套件：

   ```bash
   npm install
   ```

   ![alt text](assets/05-01-cmd-npm-install.png)

   安裝完成後會看到 `node_modules` 資料夾  
   ![alt text](assets/05-02-file-node-modules.png)

3. 打開桌面上的 Docker Desktop，接著在終端機 (cmd)執行以下指令：

   ```bash
   docker-compose up -d
   ```

   執行完後即可關閉終端機 (cmd)  
   ![alt text](assets/05-03-cmd-docker-compose_up_-d.png)

4. 打開新的終端機 (cmd)，輸入以下指令啟動後端：

   ```bash
   npm run dev:api
   ```

   後端 Port 為 8787  
   ![alt text](assets/05-04-cmd_npm_run_dev-api.png)

5. 再開一個新的終端機 (cmd)，輸入以下指令啟動前端：

   ```bash
   npm run dev
   ```

   前端 Port 為 5173  
   此時會有兩個終端機 (cmd) 視窗在運行  
   屆時造訪 [http://localhost:5173](http://localhost:5173) 即可查看前端畫面  
   注意：此時還沒有匯入「public_test_data.sql」檔案，所以會沒有資料可以顯示
   ![alt text](assets/05-05-cmd_npm_run_dev.png)

---

## 6. 啟動 pgAdmin

1. 打開終端機 (cmd)，輸入以下指令啟動本地端的 pgAdmin：

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

   ![alt text](assets/06-01-cmd-docker-pgadmin.png)

2. 執行後打開 Docker Desktop，會看到一個新的 pgAdmin 虛擬環境：

   ![alt text](assets/06-02-docker-pgadmin-virual.png)

3. 啟動新創建的虛擬環境，可透過對應的 Port 連線瀏覽器查看 GUI 介面：

- 造訪地址：[http://localhost:5051/login?next=/]([http://localhost:5051/login?next=/)
- 登入帳號：`admin@admin.com`
- 密碼：`admin`

  ![alt text](assets/06-03-01-docker-pgadmin-run.png)

  ![alt text](assets/06-03-02-localhost-pgadmin.png)

---

## 7. 匯入 SQL 測試資料

1. 請先將 `public_test_data.sql` 放入你的"下載"資料夾中。

- 使用者名稱位置  
   ![alt text](assets/07-00-01-user-name-where.png)

- 資料庫容器 ID  
   ![alt text](assets/07-00-02-docker-postgres-view.png)

1. 開啟『終端機』輸入『PowerShell』或直接開啟『PowerShell』  
   注意：輸入完指令路徑前面會多一個 PS，代表已進入 PowerShell
   ![alt text](assets/07-01-01-cmd-powershell.png)

   開始列搜索 PowerShell 直接打開
   ![alt text](assets/07-01-02-powershell.png)

2. 執行以下指令匯入資料(記得更改「你的使用者名稱」及「你的資料庫容器 ID」)：

   ```bash
   docker cp C:/Users/你的使用者名稱/Downloads/public_test_data.sql 你的資料庫容器ID:/tmp/public_test_data.sql; \
   docker exec -i 你的資料庫容器ID sh -c "pg_restore -U postgres -d shovelheroes --clean --if-exists --no-owner --no-privileges /tmp/public_test_data.sql"
   ```

   ![alt text](assets/07-02-powershell-docker-sql-input.png)

---

## 8. 連線 pgAdmin

1. 右鍵建立 Server Group 按「Create」→「Server Group」  
   ![alt text](assets/08-01-pgadmin-create-server-group.png)

2. 命名為 `shovel_heroes` →「Save」
   ![alt text](assets/08-02-pgadmin-create-server-group-input-name.png)

3. 點選新創建的 group，右鍵按「Register」→「Server」  
   ![alt text](assets/08-03-pgadmin-register-server.png)

4. General 頁籤 → Name 輸入 `shovel`  
   ![alt text](assets/08-04-pgadmin-register-server-input-general.png)

5. Connection 頁籤：

- Host：`host.docker.internal`
- Port：5432（預設）
- Maintenance：`shovelheroes`
- Username：`postgres`
- Password：`postgres`
- Save Password：打開

  ![alt text](assets/08-05-01-pgadmin-register-server-input-connection.png)

  連線成功後，可以看到「`shovel_heroes`」→「`shovel` 」下的的兩個資料庫，其中 `shovelheroes` 為主要資料庫：
  ![alt text](assets/08-05-02-pgadmin_register_show_datebase.png)

  點擊「`shovelheroes`」→「`Schemas` 」→「`Publiic`」→「`Tables`」擴展  
  如果 Tables 有數個資料表跑出，代表步驟 7 有成功匯入資料進去
  ![alt text](assets/08-05-03-pgadmin_register_show_tables.png)

  完成後，請將前後端關閉並重新啟動，造訪 [http://localhost:5173](http://localhost:5173) 即可看到後端撈取的 SQL 資料。

---

## 9. 註冊 LINE BOT

註冊連結：[https://developers.line.biz/en/docs/messaging-api/using-bot-designer/](https://developers.line.biz/en/docs/messaging-api/using-bot-designer/)

1. 點選 Login in to Console  
   ![alt text](assets/09-01-line-bot-step.png)

2. 使用 LINE 帳號登入  
   ![alt text](assets/09-02-01-line-bot-step.png)

   ![alt text](assets/09-02-02-line-bot-step.png)

3. 建立 Provider
   點擊「Create」
   ![alt text](assets/09-03-01-line-bot-step.png)

   輸入「Provider name」→「Create」
   ![alt text](assets/09-03-02-line-bot-step.png)

4. 建立 LINE Login Channel  
   點擊「專案名稱」→「Channels」→「Create a Line Login channel」
   ![alt text](assets/09-04-01-line-bot-step.png)

   Region to provide the service：`Taiwan`  
   Company or owner's country or region：`Taiwan`  
   Channel icon：`選一個圖片`  
   Channel name：`專案名稱`  
   ![alt text](assets/09-04-02-line-bot-step.png)

   Channel description：`專案描述`  
   App types：`兩個都勾起來`  
   Require two-factor authentication：`測試期間 - 關閉雙重驗證`  
   Email address：`自己的mail`  
   ![alt text](assets/09-04-03-line-bot-step.png)

   點擊「I agree to the LINE Developers Agreement」→「Create」
   ![alt text](assets/09-04-04-line-bot-step.png)

5. 取得 Channel 參數，並填入 `.env`：

- Channel ID  
   ![alt text](assets/09-05-01-line-bot-step.png)

- Channel Secret  
   ![alt text](assets/09-05-02-line-bot-step.png)

- Channel Callback URL 設定-1  
   點擊「Line Login」→「Edit」
  ![alt text](assets/09-05-03-line-bot-step.png)

- Channel Callback URL 設定-2  
   輸入「`http://localhost:8787/auth/line/callback`」→「Update」
  ![alt text](assets/09-05-04-line-bot-step.png)

- 編輯專案「根目錄」及「packages/backend」目錄中 `.env` 的 LINE 相關參數  
   ![alt text](assets/09-05-05-setting_env_line.png)

  完成後，請將前後端關閉並重新啟動，造訪 [http://localhost:5173](http://localhost:5173)即可使用登入功能。
