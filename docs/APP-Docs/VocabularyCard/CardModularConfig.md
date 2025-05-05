---
title: 模塊設定
---

## Modular Configuration (模組設定)

用於定義練習模組的屬性和行為。

題目內容會依據cardLayoutConfig.questionField 來定位，欄位替換內容 。

題目內容能夠以XML的格式來顯示不同的字卡的樣式。

`<LevelColor>`：加上標籤的文字，會根據發音評量的分數邊換顏色

`<WordMask Width="50">`：加上標籤的文字，會將整段文字遮罩起來，width = 遮罩寬度

`<CharMask Space="0" Width="50">`：加上標籤的文字，會每一個字母遮罩起來，Spacing = 每個遮罩間隔，width = 遮罩寬度

```
modularConfig (模組設定)
├── modularType: ModularType (模組類型，如發音、選擇題等)
├── question: string (題目內容)
├── answer: string (正確答案)
├── options: List<string> (選項列表，用於選擇題)
├── score: float (此題分數)
└── totalAttemptCount: int (允許嘗試次數)
```

### ModularType Enum (模組類型)

定義模組的不同類型。

```csharp
ModularType (模組類型)
├── Null (0): 無指定類型
├── Pronunciation (1): 發音練習
├── MultipleChoice (2): 選擇題
├── PairPuzzle (3): 配對拼圖
├── KeyboardSpelling (4): 鍵盤拼字
└── TrueOrFalse (5): 是非題
```