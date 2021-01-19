# Perfect Online Markdown Editor Demo [English](README.md) [简体中文](README.zh_CN.md)

<p align="center"> <a href="http://perfect.org/get-involved.html" target="_blank"> <img src="http://perfect.org/assets/github/perfect_github_2_0_0.jpg" alt="Get Involved with Perfect!" width="854" /> </a>
</p>


<p align="center"> <a href="https://github.com/PerfectlySoft/Perfect" target="_blank"> <img src="http://www.perfect.org/github/Perfect_GH_button_1_Star.jpg" alt="Star Perfect On Github" /> </a> <a href="http://stackoverflow.com/questions/tagged/perfect" target="_blank"> <img src="http://www.perfect.org/github/perfect_gh_button_2_SO.jpg" alt="Stack Overflow" /> </a> <a href="https://twitter.com/perfectlysoft" target="_blank">
        <img src="http://www.perfect.org/github/Perfect_GH_button_3_twit.jpg" alt="Follow Perfect on Twitter" /> </a> <a href="http：//w完美t.ly" target="_blank"> <img src="http://www.perfect.org/github/Perfect_GH_button_4_slack.jpg" alt="Join the Perfect Slack" /> </a>
</p>

<p align="center"> <a href="https://developer.apple.com/swift/" target="_blank"> <img src="https://img.shields.io/badge/Swift-3.0-orange.svg?style=flat" alt="Swift 3.0"> </a> <a href="https：//developer.apple.com/swift/" target="_blank"> <img src="https://img.shields.io/badge/Platforms-OS%20X%20%7C%20Linux%20-lightgray.svg?style=flat" alt="Platforms OS X | Linux"> </a> <a href="http://perfect.org/licensing.html" target="_blank"> <img src="https：//img.shields.io/badge/License-Apache-lightgrey.svg？style=flaged " alt="License Apache"> </a> <a href="http://twitter.com/PerfectlySoft" target="_blank"> <img src="https://img.shields.io/badge/Twitter-@PerfectlySoft-blue.svg?style=flat" alt="PerfectlySoft Twitter"> </a> <a href="http://perfect.ly" target="_blank"> <img src="http：//e完美t.ly/badge.svg" alt="Slack Status"> </a>
</p>

這個專案示範如何建立基於完美 HTTP 伺服器的線上 Markdown 編輯器，完美的 WebSocket 和完美的 Markdown。

請確定您已安裝並啟動最新的 Swift 3.1 工具鏈。

## 快速入門

請遵循下面的 Bash 指令來下載、建置及執行這個專案：

`$ git clone https://github.com/PerfectExamples/Perfect-Markdown-Editor.git $ cd Perfect-Markdown-編輯器 $ swift build $ ./.build/debug/PerfectMarkdownEditor`

如果成功，訊息應該會跳出如下：

`` ` [INFO] Starting HTTP server localhost on 0.0.0.0：7777 ```

這表示您可以使用瀏覽器來檢查 `http://localhost：7777`。

<p align=center><img src='sample.png'></img></p>

在本指導教學中，此專案只提供純 HTML ，而不提供任何呈現樣式，以將程式碼最小化。 不過，您可以新增任何 CSS ，使它更漂亮。

## Project Walk 直通車

整個專案都以 [PerfectTemplate]（https://github.com/PerfectlySoft/PerfectTemplate.git）為基礎。

### Package.swift

它是 Swift Package Manager 所需的一般檔案，具有伺服器元件的金鑰行：

迅速
.pPackage（url： "https：//github.com/PerfectlySoft/Perfect-HTTPServer.git"， majorVersion： 2）， .Package（url： "https：//github.com/PerfectlySoft/Perfect-WebSockets.git"， majorVersion：2）， .Package（url： "https：//github.com/PerfectlySoft/Perfect-Markdown.git"， majorVersion ： 1） ```

在這些相依關係中， _Perfect-HTTPServer_ 包括在 mac / linux 上的 Swift HTTP Server 的所有必要功能； _Perfect-Markdown_ 是一個程式庫，可讓 Swift 將 Markdown 字串轉換為 HTML；此外， _Perfect-WebSocket_ 提供 用戶端的 WebSocket 存取權，這表示任何輸入都會取得即時呈現回饋。

### main.swift

`main.swift` 是伺服器的主要項目，這是典型的 [PerfectTemplate]（https://github.com/PerfectlySoft/PerfectTemplate.git）應用程式。 伺服器只提供兩個路徑：

- `/editor` - WebSocket 處理程式，亦即，送入的 WebSocket 要求將作為下列程式來處理：

````swift public class EditorHandler： WebSocketSessionHandler {

  public let socketProtocol ： String？ = "editor"

  // This function is s呼 by the WebSocketHandler once the connection has beeonce .
  public func handleSession（request： HTTPRequest， socket： WebSocket） {

    socket.readStringMessage { input， _， _ in

      guard let input = input else {
        socket.clocks（） return }//end guard

      // translate markdown to HTML，只需一個 line code let output = input.markdownToHTML ？？ " "

		// send the HTML back to socket
      socket.sendStringMessage（string： output， final： true） {
        self.handleSession（request： request， socket： socket）
      }//end send }//end readStringMessage }//end handleSession }//end Handler ```

- `/` - 根處理程式，亦即，靜態 HTML 首頁，但用戶端使用者行為 - 鍵入任何 markdown 至輸入框，並立即轉換為 HTML - 由內嵌在 HTML中的一小部分 WebSocket Script 控制：

`` ` javascript var input， output； function init（）{
	input = document.getElementById（'textInput'）； output = document.getElementById（'results'）； // create a socket and point it to the current server with api "/editor" and protocol "editor" （可以是不同的名稱） sock = new WebSocket（'ws：//' + window.location.host + '/editor'， 'editor'）； sock.onmessage = function（evt） { output.innerText = evt.data； } }//end init function send（） {
	sockk.send（input.value）； } ```

## 問題

我們正在為所有錯誤使用 JIRA 進行轉移，並支援相關的問題，因此已停用 GitHub 問題。

如果您發現錯誤、錯誤或您想在文件上提出的任何其他有用建議，請前往 [http://jira.perfect.org：8080/servicedesk/customer/portal/1]（http://jira.perfect.org：8080/servicedesk/customer/portal/1），並將其調高。

[ http://jira.perfect.org：8080/projects/ISS/ 問題 ] （ http://jira.perfect.org：8080/projects/ISS/ 問題），可以在 [ http://jira.perfect.org：8080/projects/ISS/ 問題 ] 中找到一份完整的開放問題清單。

## 關於完美專案的更多資訊，請訪問 [perfect.org ] （ http://perfect.org ）。

## Now WeChat Subscription is Available （中文）
<p align=center><img src="https://raw.githubusercontent.com/PerfectExamples/Perfect-Cloudinary-ImageUploader-Demo/master/qr.png"></p>

````
