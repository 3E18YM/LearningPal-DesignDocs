---
title: 字卡內容
---

## T Content  (泛型內容)

內容分為單字 (Vocabulary) 和對話 (Conversation)。

### Vocabulary (單字內容)

定義單字相關的屬性與詳細資訊。

```csharp
Vocabulary (單字內容)
├── ID: string (單字唯一識別碼)
├── Word: string (單字)
├── Translation: string (翻譯)
├── Phonetic: string (音標)
├── Description: string (單字描述)
├── Sentence (例句)
│   ├── Content: string (句子內容)
│   └── Translation: string (句子翻譯)
└── ImageUrl: string (圖片路徑)

```

### Conversation (對話內容)

定義對話的結構與內容。

```csharp
Conversation (對話內容)
├── Title: string (對話標題)
├── Description: string (對話描述)
├── Names: List<string> (對話者名稱列表)
├── Sentences: List<Sentence> (對話句子列表)
│   ├── Content: string (句子內容)
│   └── Translation: string (句子翻譯)
└── ImageUrl: string (對話情境圖片)

```