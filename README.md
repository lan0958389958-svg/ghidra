<img src="Ghidra/Features/Base/src/main/resources/images/GHIDRA_3.png" width="400">

# Ghidra 軟體逆向工程框架
Ghidra 是一個由 [國家安全局][nsa] 研究部門創建和維護的軟體逆向工程 (SRE) 框架。此框架包含一套功能齊全、高階的軟體分析工具，使用戶能夠在包括 Windows、macOS 和 Linux 在內的多種平台上分析編譯後的程式碼。其功能包括反彙編、彙編、反編譯、圖形化和腳本編寫，以及數百種其他功能。Ghidra 支援多種處理器指令集和可執行檔格式，並可在使用者互動和自動化模式下運行。使用者還可以利用 Java 或 Python 開發自己的 Ghidra 擴充元件和/或腳本。

為支援國家安全局的網路安全任務，Ghidra 旨在解決複雜 SRE 工作中的規模和團隊協作問題，並提供一個可客製化和可擴展的 SRE 研究平台。國家安全局已將 Ghidra SRE 功能應用於各種問題，包括分析惡意程式碼，並為尋求更深入了解網路和系統潛在漏洞的 SRE 分析師提供深入見解。

如果您是美國公民，對此類專案感興趣，希望為國家安全局開發 Ghidra 和其他網路安全工具，以幫助保護我們的國家及其盟友，請考慮申請我們的 [職位][career]。

## 安全警告
**警告：** 某些版本的 Ghidra 存在已知的安全漏洞。在繼續之前，請閱讀 Ghidra 的 [安全公告][security]，以更好地了解您可能受到的影響。

## 安裝
要安裝官方預建的多平台 Ghidra 版本：
* 安裝 [JDK 21 64 位元][jdk]
* 下載 Ghidra [發行檔案][releases]
  - **注意：** 官方多平台發行檔案的名稱為 `ghidra_<version>_<release>_<date>.zip`，可在「Assets」下拉選單中找到。下載名為「Source Code」的任何檔案都是不正確的。
* 解壓縮 Ghidra 發行檔案
  - **注意：** 請勿在現有安裝之上解壓縮
* 啟動 Ghidra：`./ghidraRun` (Windows 為 `ghidraRun.bat`)
  - 或啟動 [PyGhidra][pyghidra]：`./support/pyghidraRun` (Windows 為 `support\pyghidraRun.bat`)

有關安裝和運行 Ghidra 版本的更多資訊和故障排除提示，請參閱 Ghidra 安裝目錄根目錄下的 [入門文件][gettingstarted]。

## 建置
[![建置 Ghidra][build-ghidra-badge]][build-ghidra-action]

要從此原始碼儲存庫為您的平台建立最新的開發建置：

##### 安裝建置工具：
* [JDK 21 64 位元][jdk]
* [Gradle 8.5+][gradle] (如果網路連線可用，則使用提供的 Gradle 包裝器)
* [Python3][python3] (版本 3.9 至 3.14) 附帶 pip
* GCC 或 Clang，以及 make (僅限 Linux/macOS)
* [Microsoft Visual Studio][vs] 2017+ 或 [Microsoft C++ 建置工具][vcbuildtools] 並安裝以下元件 (僅限 Windows)：
  - MSVC
  - Windows SDK
  - C++ ATL

##### 下載並解壓縮原始碼：
[從 GitHub 下載][master]
```
unzip ghidra-master
cd ghidra-master
```
**注意：** 您可能希望克隆 GitHub 儲存庫，而不是下載壓縮的原始碼：`git clone https://github.com/NationalSecurityAgency/ghidra.git`

##### 將額外的建置依賴項下載到原始碼儲存庫中：
**注意：** 如果網路連線可用且您未安裝 Gradle，則在以下說明中，`./gradlew` (或 `gradlew.bat`) 命令可用於替代 `gradle` 命令。

```
gradle -I gradle/support/fetchDependencies.gradle
```

##### 建立開發建置：
```
gradle buildGhidra
```
壓縮的開發建置將位於 `build/dist/`。

有關建置 Ghidra 的更詳細資訊，請閱讀 [開發人員指南][devguide]。

對於建置問題，請查看 [已知問題][known-issues] 部分以獲取可能的解決方案。

## 開發

### 使用者腳本和擴充功能
Ghidra 安裝支援使用者透過 Eclipse 的 *GhidraDev* 外掛程式編寫自訂腳本和擴充功能。該外掛程式及其相應說明可在 Ghidra 版本中的 `Extensions/Eclipse/GhidraDev/` 或 [此連結][ghidradev] 找到。或者，可以透過點擊腳本管理器中的 Visual Studio Code 圖示來使用 Visual Studio Code 編輯腳本。功能齊全的 Visual Studio Code 專案可以從 Ghidra CodeBrowser 視窗中的 _工具 -> 建立 VSCode 模組專案_ 建立。

**注意：** Eclipse 的 *GhidraDev* 外掛程式和 Visual Studio Code 整合都只支援針對從 [發行頁面][releases] 下載的完整建置 Ghidra 安裝進行開發。

### 進階開發
要開發 Ghidra 工具本身，強烈建議使用 Eclipse，Ghidra 的開發過程已針對其進行高度客製化。

##### 安裝建置和開發工具：
* 遵循上述 [建置說明](#build)，確保建置無錯誤完成
* 安裝 [Eclipse IDE for Java Developers][eclipse]

##### 準備開發環境：
```
gradle prepdev eclipse buildNatives
```

##### 將 Ghidra 專案匯入 Eclipse：
* *檔案* -> *匯入...*
* *一般* | *現有專案到工作區*
* 選擇根目錄為您下載或克隆的 ghidra 原始碼儲存庫
* 勾選 *搜尋巢狀專案*
* 點擊 *完成*

當 Eclipse 完成建置專案後，Ghidra 可以使用提供的 **Ghidra** Eclipse *運行配置* 啟動和調試。

有關開發 Ghidra 的更詳細資訊，請閱讀 [開發人員指南][devguide]。

## 貢獻
如果您想為 Ghidra 貢獻錯誤修復、改進和新功能，請查看我們的 [貢獻者指南][contrib]，了解如何參與此開源專案。

[build-ghidra-action]: https://github.com/NationalSecurityAgency/ghidra/actions/workflows/build-ghidra.yml
[build-ghidra-badge]: https://github.com/NationalSecurityAgency/ghidra/actions/workflows/build-ghidra.yml/badge.svg
[nsa]: https://www.nsa.gov
[contrib]: CONTRIBUTING.md
[devguide]: DevGuide.md
[gettingstarted]: GhidraDocs/GettingStarted.md
[known-issues]: DevGuide.md#known-issues
[career]: https://www.intelligencecareers.gov/nsa
[releases]: https://github.com/NationalSecurityAgency/ghidra/releases
[jdk]: https://adoptium.net/temurin/releases
[gradle]: https://gradle.org/releases/
[python3]: https://www.python.org/downloads/
[vs]: https://visualstudio.microsoft.com/vs/community/
[vcbuildtools]: https://visualstudio.microsoft.com/visual-cpp-build-tools/
[eclipse]: https://www.eclipse.org/downloads/packages/
[master]: https://github.com/NationalSecurityAgency/ghidra/archive/refs/heads/master.zip
[security]: https://github.com/NationalSecurityAgency/ghidra/security/advisories
[ghidradev]: GhidraBuild/EclipsePlugins/GhidraDev/GhidraDevPlugin/README.md
[pyghidra]: Ghidra/Features/PyGhidra/README.md
```
