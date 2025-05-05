---
title: 排版設定
---
## CardLayout Configuration (卡片布局設定)

用於定義卡片的外觀與內容。

```csharp
cardLayoutConfig (卡片布局設定)
├── contents: List<CardContent(enum)> (卡片上要顯示的欄位列表)
└── questionField: CardContent(enum) (問題欄位名稱，用於定位問題內容)

```

### CardContent (enum) (卡片內容參數)

C# Enum 用於對應卡片顯示欄位。

```csharp
CardContent (卡片內容)
├── Null (0): 無內容
├── image (1): 圖片
├── text (2): 文字
├── audio (3): 音訊
├── phonetic (4): 音標
├── translation (5): 翻譯
├── description (6): 描述
├── sentence (7): 句子
└── practiceModular (8): 練習模組

```