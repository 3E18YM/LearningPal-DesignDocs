---
title: 練習組合 PracticeType
---

## 練習組合 PracticeType

LearningPal 可以基於特定的練習類型，來透過不同的模板和模塊`CardLayoutConfig` `ModularConfig` ，以產生不同的學習需求。這些練習類型定義在 `PracticeType` 枚舉中：

```csharp
public enum PracticeType
{
    Null = -1,
    SeeAndSay = default,    // 看圖說話
    ReadItOut,              // 朗讀單詞
    ReadSentence,           // 朗讀句子
    Listening,              // 聽力練習
    SeeAndSpelling,         // 看圖拼寫
    SpellingWord,           // 單詞拼寫
    SentenceSpelling,       // 句子拼寫
    PuzzleSpelling,         // 拼圖拼寫
    PuzzleSentence,         // 句子拼圖
    MultipleChoice,         // 多選題
    SeeAndChoose,           // 看圖選擇
    ChooseItRight,          // 選擇正確答案
    SentenceChoose,         // 句子選擇
    TrueOrFalse,            // 真假題
    ConversationRepeat,     // 對話重複
    ConversationAnswer,     // 對話回答
    Custom                  // 自定義練習
}
```

### 練習類型的模塊和模板對照


| 練習類型 | 模組類型 |模板類型| 內容類型 | 需要選項 | 適用場景 |
|---------|---------|---------|---------|---------|---------|
| SeeAndSay | 發音 |圖片+文字| 詞彙 | 否 | 初學者詞彙學習 |
| ReadItOut | 發音 |單字內容| 詞彙 | 否 | 詞彙發音練習 |
| ReadSentence | 發音 |單字+例句| 句子 | 否 | 句子朗讀練習 |
| Listening | 多選題 |圖片+文字| 詞彙 | 是 | 聽力理解練習 |
| SeeAndSpelling |模板類型| 圖片+文字 | 詞彙 | 否 | 詞彙拼寫練習 |
| SpellingWord | 鍵盤拼寫 |單字內容| 詞彙 | 否 | 詞彙拼寫練習 |
| SentenceSpelling | 鍵盤拼寫 |單字+例句| 句子 | 否 | 句子拼寫練習 |
| PuzzleSpelling | 配對拼圖 |單字內容| 詞彙 | 否 | 互動式詞彙拼寫 |
| PuzzleSentence | 配對拼圖 |單字+例句| 句子 | 是 | 互動式句子組合 |
| MultipleChoice | 多選題 |圖片+文字| 詞彙 | 是 | 詞彙理解測試 |
| SeeAndChoose | 多選題 |圖片+文字| 詞彙 | 是 | 圖像詞彙匹配 |
| ChooseItRight | 多選題 |單字內容| 詞彙 | 是 | 詞彙選擇練習 |
| SentenceChoose | 多選題 |單字+例句| 句子 | 是 | 句子理解測試 |
| TrueOrFalse | 是非題 | 圖片+文字 |詞彙| 是 | 句子理解測試 |
| ConversationRepeat | 發音 |尚未規劃| 對話 | 否 | 對話發音練習 |
| ConversationAnswer | 尚未規劃 |尚未規劃| 對話 | 是 | 對話理解測試 |