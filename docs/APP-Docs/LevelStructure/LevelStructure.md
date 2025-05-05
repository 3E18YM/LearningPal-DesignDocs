---
title: 關卡資料結構
---

# 主架構
<pre>
```csharp
CourseConfig (課程設定檔)
├── Unit: string // 單元名稱
├── Name: string // 課程名稱
├── ID: string // 系統自動生成的唯一識別碼
├── Description: string // 課程描述
│
└── LevelData: List<PracticeLevel> // 課程中的所有關卡列表
    └── PracticeLevel (關卡資料)
        ├── ID: string // 關卡唯一識別碼
        ├── levelName: string // 關卡名稱
        ├── practiceType: PracticeType(enum) // 練習類型
        ├── level: LevelDiff(enum) // 難度等級
        ├── contentType: ContentType(enum) // 內容類型
        ├── properties: Dictionary&ltstring, object&lt // 關卡額外屬性，存入DB序列換成純文字
        │   ├── rating: int // 星級評分
        │   ├── completion: int // 完成度
        │   └── totalCounts: int // 總練習次數
        │
        └── practiceQuests: List<PracticeQuest> // 練習任務列表
            └── PracticeQuest (練習任務)
                ├── ID: string // 任務唯一識別碼
                ├── practiceType: PracticeType // 練習類型              
                ├── level: LevelDiff // 難度等級               
                ├── contentType: ContentType // 內容類型              
                └── contentIDs: List<int> // 內容ID列表
```
</pre>

# PracticeLevel (關卡)

- 每個關卡都有唯一 ID 和名稱
- 包含多個練習題目 (PracticeQuest)
- 可以設定預設的練習類型和難度，當PracticeQuest 未設定練習練習或難度時將使用此參數
- 預設類型以及難度也會是關卡選擇器上的UI 圖示、顏色判定值

```csharp
PracticeLevel (關卡資料)
  ├── ID: string // 關卡唯一識別碼
  ├── levelName: string // 關卡名稱
  ├── practiceType: PracticeType(enum) // 預設練習類型
  ├── level: LevelDiff(enum) // 預設難度等級
  ├── contentType: ContentType(enum) // 內容類型
  ├── properties: Dictionary<string, object> // 關卡額外屬性，存入DB序列換成純文字
  └── practiceQuests: List<PracticeQuest> // 練習任務列表
```

### PracticeType Enum (練習類型)

- 依照不同的enum ，APP 將自動生成不同的模板和練習模塊設定

```csharp
PracticeType (練習類型)
├── Null (-1): 無指定類型
├── SeeAndSay: 看圖說話(發音評量)
├── ReadItOut: 看字念字(發音評量)
├── ReadSentence: 讀句子(發音評量)
├── SeeAndSpelling: 看圖拼寫 (鍵盤拼字，逐字母遮罩)
├── SpellingWord: 拼單字 (鍵盤拼字，逐單字遮罩)
├── SentenceSpelling: 拼句子 (鍵盤拼出句子中單字，逐單字遮罩)
├── PuzzleSpelling: 單字拼圖(逐字母拼圖配對)
├── PuzzleSentence: 句子重組 (句子中隨機單字拼圖，可設定挖空數量)
├── SeeAndChoose: 看圖選擇 (選擇題看圖選單字，從PracticeLevel 中所有單字隨機抽取)
├── ChooseItRight: 選擇正確答案 (選擇題單字欄位挖空選字)
├── SentenceChoose: 選擇句子 (選擇題例句挖空選字)
├── TrueOrFalse: 是非題(圖片和單字是否匹配)
└── Custom: 自定義
```

### LevelDiff Enum (難度等級)

定義練習關卡的難度等級。

```csharp
LevelDiff (難度等級)
├── Easy: 簡單
├── Medium: 中等
├── Advanced: 高級
├── Expert: 專家
└── Master: 大師
```

### ContentType Enum (內容類型)

```csharp
ContentType(難度等級)
├── Vocabulary: 單字
└── Conversation: 對話
```

## `Dictionary<string, object>` Properties (額外屬性)

以Json 方式序列化`Dictionary<string , object>` 儲存的額外內容，需要時才會解包。

**目前使用參數：**

用來儲存使用者再此關卡的最高分數、完成度、字卡數量等.…

此資料用來提供關卡選擇頁面顯示星星評比以及完成度

```csharp
properties: Dictionary<string, object> // 關卡額外屬性，存入DB序列換成純文字
  ├── rating: int // 星級評分
  ├── completion: int // 完成度
  └── totalCounts: int // 總練習次數
```

# PracticeQuest (練習題目)

- 在同一個關卡中可以包含不同類型的練習及難度
- Quest 會依據contentType、和ContentIDs 來取得Vocabulary 或Conversation等
- 依照ID 取得內容後，會為每個內容根據設定生成字卡、對話等練習

```csharp
PracticeQuest (練習任務)
  ├── ID: string // 任務唯一識別碼
  ├── practiceType: PracticeType // 練習類型              
  ├── level: LevelDiff // 難度等級               
  ├── contentType: ContentType // 內容類型              
  └── contentIDs: List<int> // 內容ID列表
```