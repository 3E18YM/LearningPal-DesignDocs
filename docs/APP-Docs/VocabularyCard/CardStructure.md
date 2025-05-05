---
title: 字卡基本架構
---
## Properties(額外屬性)

以Json 方式序列化`Dictionary<string , object>` 儲存的額外內容，在需要時才會解包。

```csharp
properties: Dictionary<string, object> (額外屬性字典)
├── LevelID: string (關卡ID)
├── QuestIndex: int (題目索引)
├── PracticeType: PracticeType (練習類型)
└── LevelDiff: LevelDiff (難度等級)

```