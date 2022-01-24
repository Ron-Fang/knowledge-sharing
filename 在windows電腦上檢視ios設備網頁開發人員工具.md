[原文網址]( https://juejin.cn/post/6982738741332803592#heading-18)
### 第一步 : 安裝iTune並登入
1.[iTune for Windows](https://support.apple.com/zh-tw/HT210384)
### 第二步 : 在ios上打開網頁檢閱器
1. 網頁檢閱器位置 : 設定 >> safari >> 進階 >> 打開網頁檢閱器
### 第三步 : 在windows上安裝scoop套件管理工具
1. 打開PowerShell(windows+R然後輸入PowerShell) >> 下scoop安裝指令(指令如下)
```js
iex (new-object net.webclient).downloadstring('https://get.scoop.sh')
```
2. 如果未安裝成功，請再依次執行下列指令
```js
Set-ExecutionPolicy RemoteSigned -scope CurrentUser // 修改执行策略
```
```js
iex (new-object net.webclient).downloadstring('https://get.scoop.sh')
```
3. 如果還是安裝失敗，再依次執行下列指令
```js
iex (new-object net.webclient).downloadstring('https://raw.githubusercontent.com/lukesampson/scoop/master/bin/install.ps1')
```
```js
scoop update
```
4. 如果還是失敗可能就是網速太慢安裝未完成，需刪除scoop再重新執行第三項步驟
### 第四步 : 通過scoop安装 ios-webkit-debug-proxy
1. 打開PowerShell >> 下ios-webkit-debug-proxy安裝指令(指令如下)
```js
scoop bucket add extras // 新增資料夾
```
```js
scoop install ios-webkit-debug-proxy
```
2. 如安裝失敗可能是網速慢、scoop版本舊或scoop沒安裝好，可用以下指令確認scoop是否安裝成功
```js
scoop help //有跳出一堆scoop推薦指令就是成功
```
3.可用以下指令檢查版本
```js
scoop status //如顯示Scoop is already installed Run 'scoop update' to get the latest version. 則須執行第二步第三項第二個指令，更新版本
```
### 第五步 : 啟動ios-webkit-debug-proxy(本教學使用google chrome為網頁檢查工具)
1. 將手機設備連接電腦(須注意USB孔電壓是否充足、連接線是否為原廠)
2. 打開PowerShell >> 下啟動指令(-f表指定前端工具)
```js
ios_webkit_debug_proxy -f chrome-devtools://devtools/bundled/inspector.html
```
### 第六步 : 檢查網頁連線是否成功，並開始監看網頁
1. 打開google chrome在網址列輸入:
 
	localhost:9221 //成功連線會顯示設備名稱

2. 打開手機safari到想監看的網頁，在電腦google chrome新分頁中輸入下列網址:

	chrome://inspect/#devices

3. 等待一小段時間他會抓取手機網頁訊號，如下圖出現你的手機網頁名稱，即可點inspect開始執行開發者模式，用以檢查network等網頁資訊
 ![[Image 1.png]]
4. 如果點擊inspect打開新視窗，未顯示手機畫面，請關閉powershell並執行以下指令
	* 安装 vs-libimobile
	 ```js
	npm install -g vs-libimobile
	```
	* 安装最新版本的適配器
	```js
	npm install remotedebug-ios-webkit-adapter -g
	```
5. 重新確認手機連線並執行以下指令
	```js
	remotedebug_ios_webkit_adapter --port=9222// ios-webkit-debug-proxy 將自動啟動
	```
	* 下完啟動指令後一樣檢查是否連線成功，並刷新chrome://inspect/#devices頁面，再點擊inspect執行網頁監測。
	* 補充:如果還是沒有畫面有可能是port被佔用問題，可以換9000或其他port試試看，換port記得要在chrome://inspect/#devices頁面的Configure...按鈕設定新的port位
### 後記
1. hrome也會偶爾檢測不到或者比較卡的情況，這時候可能需要在powershell重下啟動指令remote debug-ios-webkit-adapter，或是先嘗試關掉檢查網頁再點擊inspect開啟
2. 有時會發生監測視窗閃退為正常狀況，再重新開視窗即可。