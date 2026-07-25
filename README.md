# KK_PerspectiveX

一款適用於 Koikatsu 的第一人稱 POV 插件，不會讓鏡頭穿進角色身體，也不會讓你感到暈眩。

支援主遊戲（自由行動模式）、H 場景以及 CharaStudio。

![POV 模式](img/pov.png)

同一個 DLL 同時支援 Koikatsu 與 Koikatsu Sunshine。我已經在兩款遊戲中遊玩並測試了很長一段時間，目前兩者的運作方式相同。不過，Sunshine 是較新的版本，而且在 POV 模組中比較少見，因此偶爾可能會出現 Sunshine 專屬的問題。如果未來兩款遊戲的差異變得足夠大，我會將插件拆分成不同版本；但目前同一個 DLL 就能正常支援兩款遊戲。

由 Tokozakura 維護的繁體中文（ZH-TW）翻譯分支可在此取得：[releases](https://github.com/Tokozakura/KK_PerspectiveX_ZH-TW/releases)。

## 為什麼還要再做一款 POV 模組？

因為現有的 POV 模組一直有一些讓我感到困擾的問題。有些模組會把鏡頭放在錯誤的位置，導致視角位於胸口高度，而且頭部還會穿進畫面。RealPOV 雖然能正確放置鏡頭，但它會完整複製頭部骨骼的動畫旋轉，包括側傾旋轉，因此視角會隨著每個動畫向側面傾斜並劇烈晃動。PerspectiveX 會將位置與旋轉分開處理：

|                 | PerspectiveX                                                          | RealPOV                                        | KK_StudioPOV（僅限 Studio）                                               |
| --------------- | --------------------------------------------------------------------- | ---------------------------------------------- | --------------------------------------------------------------------- |
| **視點位置**        | 真正位於眼睛高度，設置在實際雙眼骨骼的中心點                                                | 同樣位於雙眼骨骼的中心點                                   | 同樣位於雙眼骨骼的中心點                                                          |
| **旋轉方式**        | 完全由滑鼠控制，並且不受角色骨骼影響，地平線會始終保持水平。可選擇啟用「動畫擺動」，讓可調整比例的頭部動作影響鏡頭，但側傾旋轉始終會被移除 | 會覆寫頸部骨骼，但鏡頭仍然會繼承頭部完整的世界旋轉，因此動畫中的脊椎或胸部傾斜仍會讓視角側傾 | 每一幀都會複製眼睛骨骼的原始旋轉，因此任何動畫或姿勢調整都會直接旋轉鏡頭，而且俯仰角沒有任何限制                      |
| **POV 模式下調整姿勢** | 移動骨骼時，例如使用 KKPE，不會讓視角跟著旋轉，鏡頭方向完全由你控制                                  | 調整上層骨骼時仍可能讓鏡頭傾斜，因為旋轉並未完全分離                     | 骨骼調整會立即移動鏡頭，因為它會即時讀取骨骼旋轉                                              |
| **隱藏頭部**        | 只隱藏頭部的渲染器：頭部會從畫面中消失，但仍會正常播放動畫，而且不會將任何資料寫入角色卡或場景檔案                     | 切換角色儲存資料中的「頭部永遠顯示」旗標（可選功能，預設關閉）                | 停用整個頭部物件；由於 Unity 會停止播放已停用骨骼的動畫，眼睛位置可能會固定不動，但身體仍會繼續移動，導致鏡頭逐漸偏移並穿進角色身體 |
| **滑鼠視角限制**      | 乾淨的水平與垂直旋轉，不會發生角度循環                                                   | 旋轉累積沒有角度限制                                     | 旋轉累積沒有角度限制，拖曳過多甚至可能讓畫面上下顛倒                                            |
| **舒適性選項**       | 位置平滑、動畫擺動混合比例、前後與上下偏移、即時 FOV、一鍵預設、可儲存視角欄位、鏡頭鎖定，所有設定都能在 POV 模式中即時調整    | FOV、靈敏度、固定偏移                                   | FOV、靈敏度                                                               |

（以上內容是根據 [RealPOV](https://github.com/Keelhauled/KeelPlugins) 與 [KK_StudioPOV](https://github.com/Mantas-2155X/StudioPOV) 的公開原始碼進行分析，只是在說明程式碼的實際運作方式，並不是在批評任何一個專案。）

## 安裝方式

1. 你需要安裝 BepInEx 5（HF Patch 已經內建）。
2. 從 [Releases](../../releases) 下載 `KK_PerspectiveX.dll`，並將它放入 `BepInEx/plugins/`。
3. 如果你已經安裝 RealPOV（我記得 HF Patch 有內建），請將 `RealPOV.Koikatu.dll` 重新命名為 `RealPOV.Koikatu.dll.disabled` 來停用它，因為兩款插件都使用 Backspace 作為切換按鍵。

本插件已在 KK 與 KKS、BepInEx 5 以上版本中實際測試及遊玩。如果無法在你的環境中正常運作，請[建立問題回報](../../issues)，我會協助檢查。

## 操作方式

所有按鍵都可以透過 ConfigurationManager（F1）重新綁定。

* **Backspace**：切換 POV 模式。在 Studio 中，請先在工作區選取一名角色。
* **滑鼠左鍵（按住並拖曳）**：環顧四周。其他時候滑鼠游標仍會保持可見並可正常操作。
* **Ctrl+L**：啟用免按鍵的 FPS 滑鼠視角。滑鼠游標會被鎖定，直到再次按下此快捷鍵。
* **Ctrl+Shift+Left / Ctrl+Shift+Right**：將 POV 切換至上一名或下一名角色。
* **滑鼠滾輪**：在 POV 模式中調整 FOV（如果經常不小心誤觸，可以在設定中停用）。
* **Comma / Period**：讓鏡頭向左或向右傾斜。**Slash** 可將傾斜角度重設為水平。
* **Semicolon**：將鏡頭鎖定在原地。視角會停止跟隨頭部，適合用於會讓頭部大幅晃動的愛撫動畫；鎖定期間仍然可以自由觀看。再次按下即可解除鎖定，鏡頭會平滑返回原位。
* **Ctrl+Shift+1/2/3**：將目前視角（FOV、觀看方向、傾斜角度、鏡頭偏移）儲存至指定欄位。**Ctrl+1/2/3**：讀取已儲存的視角。儲存欄位會保留至下一次啟動遊戲。
* **Ctrl+P**：開啟預設面板。在 Studio 中，也可以點擊左下角工具列中的 PerspectiveX 眼睛按鈕。

### 預設面板

遊戲內面板會將所有與預設相關的功能集中在同一個位置，不需要進入設定檔調整：

* **視角預設**：提供一鍵套用的設定（Cozy 60 / Natural 90 / Action 110），會同時設定 FOV、位置平滑程度與鏡頭偏移。
* **自訂預設**：調整自己喜歡的 FOV、平滑程度與偏移組合，輸入名稱後儲存成自訂的一鍵預設（最多 5 組）。
* **已儲存視角**：以按鈕形式提供與快捷鍵相同的三組儲存及讀取欄位，並顯示每個欄位的 FOV。

舒適性設定，包括位置平滑、動畫擺動、FOV 與偏移，都位於插件設定中，並且可以在 POV 模式中即時調整。視角預設也可以在設定中使用。可選的「Align camera with body」設定會讓視角配合角色身體的方向傾斜，例如角色側躺時，鏡頭也會跟著側躺，而不是始終保持地平線水平。

## 從原始碼建置

本插件以 .NET Framework 3.5 為目標框架，可以在任何作業系統中使用一般的 .NET SDK 進行建置。

首先，請從遊戲安裝目錄將以下參考 DLL 複製到 `lib/`。這些檔案受版權保護，因此未包含在此處：

* `BepInEx/core/`：`BepInEx.dll`、`0Harmony.dll`
* `Koikatu_Data/Managed/`：`Assembly-CSharp.dll`、`Assembly-CSharp-firstpass.dll`、`UnityEngine.dll`、`UnityEngine.UI.dll`

接著執行：

```sh
cd src/KK_PerspectiveX
dotnet build -c Release
```

輸出位置：`src/KK_PerspectiveX/bin/Release/KK_PerspectiveX.dll`

## XMods 系列

* [KK_PerspectiveX](https://github.com/bani4kaskashka/KK_PerspectiveX)：不會穿進角色身體，也不會讓你感到暈眩的第一人稱 POV 插件（本插件）
* [StudioHeightX](https://github.com/bani4kaskashka/KK-KKS-StudioHeightX)：即時顯示目前在 Studio 中選取角色的身高
* [PerceptiveHeightX](https://github.com/bani4kaskashka/PerceptiveHeightX)：PerspectiveX 的 POV 體型感知附加插件

## 我的連結

* [Patreon](https://www.patreon.com/c/tokozakura_studio)
* [pixiv,net](https://www.pixiv.net/users/34631297)

## 授權條款

[MIT](LICENSE)
