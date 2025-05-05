---
title: 練習類型 Practice Types
sidebar_position: 1
---

# 練習類型

LearningPal 系統提供多種練習類型，以滿足不同的學習需求和場景。本文檔詳細介紹了這些練習類型及其實現方式。

## 練習類型概述

LearningPal 支持以下主要練習類型：

1. **語音練習**：包括朗讀單詞、朗讀句子和對話重複
2. **拼寫練習**：包括單詞拼寫、句子拼寫和拼圖拼寫
3. **選擇練習**：包括多選題、看圖選擇和真假題
4. **聽力練習**：包括聽力理解和聽寫練習
5. **對話練習**：包括對話重複和對話回答

每種練習類型都有其特定的教學目標和評估標準，可以根據學習內容和難度級別進行配置。

## 語音練習

語音練習主要訓練用戶的發音能力，使用 Azure Speech Services 進行語音識別和評估。

### 朗讀單詞 (ReadItOut)

```csharp
public class ReadItOutModular : IPracticeModular
{
    public override void Initial<T>(T content)
    {
        // 初始化朗讀單詞練習
        // 設置語音識別參數
        // 準備評分標準
    }
    
    public override void Run()
    {
        // 開始錄音
        // 監聽用戶語音輸入
    }
    
    public override void Stop()
    {
        // 停止錄音
        // 評估用戶發音
        // 生成結果
    }
}
```

朗讀單詞練習要求用戶朗讀顯示的單詞，系統會評估發音的準確性和流暢度。主要特點包括：

- **即時反饋**：用戶朗讀後立即獲得評分和反饋
- **發音指導**：提供標準發音供用戶參考
- **多次嘗試**：允許用戶多次嘗試以提高分數

### 朗讀句子 (ReadSentence)

朗讀句子練習擴展了朗讀單詞的功能，要求用戶朗讀完整的句子。系統會評估發音、語調和流暢度。主要特點包括：

- **句子分段**：將長句子分成多個部分進行評估
- **語調評估**：評估用戶的語調是否自然
- **關鍵詞強調**：突出顯示句子中的關鍵詞

### 對話重複 (ConversationRepeat)

對話重複練習要求用戶模仿對話中的句子。系統會播放對話，然後用戶需要重複特定角色的台詞。主要特點包括：

- **角色扮演**：用戶扮演對話中的一個角色
- **情境學習**：在真實對話情境中練習語言
- **連續評估**：對整個對話過程進行評估

## 拼寫練習

拼寫練習主要訓練用戶的拼寫能力，通過不同的交互方式提高學習興趣。

### 單詞拼寫 (SpellingWord)

```csharp
public class SpellingWordModular : IPracticeModular
{
    public override void Initial<T>(T content)
    {
        // 初始化單詞拼寫練習
        // 生成字母按鈕
        // 設置評分標準
    }
    
    public override void Run()
    {
        // 顯示拼寫提示
        // 監聽用戶輸入
    }
    
    public override void Stop()
    {
        // 評估拼寫結果
        // 生成結果
    }
}
```

單詞拼寫練習要求用戶拼寫顯示的單詞或與圖片相關的單詞。主要特點包括：

- **字母提示**：可以顯示部分字母作為提示
- **錯誤反饋**：即時顯示拼寫錯誤
- **自動糾正**：提供拼寫建議

### 句子拼寫 (SentenceSpelling)

句子拼寫練習要求用戶拼寫完整的句子。系統會提供部分單詞或結構作為提示。主要特點包括：

- **單詞提示**：顯示部分單詞作為提示
- **結構指導**：提供句子結構指導
- **漸進難度**：隨著用戶進步增加難度

### 拼圖拼寫 (PuzzleSpelling)

拼圖拼寫練習將單詞或句子拆分成多個部分，用戶需要將它們重新組合。主要特點包括：

- **拖放交互**：通過拖放操作重組單詞或句子
- **視覺反饋**：提供視覺反饋指示正確位置
- **遊戲化學習**：將學習過程遊戲化

## 選擇練習

選擇練習主要訓練用戶的理解能力和詞彙記憶，通過多種選擇題型提高學習效果。

### 多選題 (MultipleChoice)

```csharp
public class MultipleChoiceModular : IPracticeModular
{
    public override void Initial<T>(T content)
    {
        // 初始化多選題練習
        // 生成選項按鈕
        // 設置評分標準
    }
    
    public override void Run()
    {
        // 顯示問題和選項
        // 監聽用戶選擇
    }
    
    public override void Stop()
    {
        // 評估選擇結果
        // 生成結果
    }
}
```

多選題練習要求用戶從多個選項中選擇正確答案。主要特點包括：

- **多種題型**：支持單選題和多選題
- **隨機排序**：選項順序隨機排列
- **即時反饋**：選擇後立即顯示正確答案

### 看圖選擇 (SeeAndChoose)

看圖選擇練習顯示一張圖片，要求用戶選擇與圖片相關的正確選項。主要特點包括：

- **圖文結合**：將圖片與文字結合
- **情境理解**：訓練用戶在情境中理解語言
- **視覺記憶**：強化視覺與語言的連接

### 真假題 (TrueOrFalse)

真假題練習顯示一個陳述，要求用戶判斷其真假。主要特點包括：

- **簡單交互**：只需選擇真或假
- **批判性思考**：訓練用戶的批判性思考能力
- **快速反應**：適合快速複習和測試

## 聽力練習

聽力練習主要訓練用戶的聽力理解能力，通過不同的聽力任務提高語言輸入能力。

### 聽力理解 (Listening)

```csharp
public class ListeningModular : IPracticeModular
{
    public override void Initial<T>(T content)
    {
        // 初始化聽力練習
        // 準備音頻資源
        // 設置評分標準
    }
    
    public override void Run()
    {
        // 播放音頻
        // 顯示問題和選項
        // 監聽用戶回答
    }
    
    public override void Stop()
    {
        // 評估回答結果
        // 生成結果
    }
}
```

聽力理解練習播放音頻，然後要求用戶回答相關問題。主要特點包括：

- **多種音頻**：包括單詞、句子和對話
- **控制播放**：用戶可以控制音頻播放
- **多種問題**：包括選擇題、填空題等

### 聽寫練習 (Dictation)

聽寫練習播放音頻，要求用戶寫下聽到的內容。主要特點包括：

- **分段播放**：可以分段播放長音頻
- **重複聆聽**：允許多次聆聽
- **部分提示**：可以提供部分文字作為提示

## 對話練習

對話練習主要訓練用戶的交流能力，通過模擬真實對話場景提高語言應用能力。

### 對話回答 (ConversationAnswer)

```csharp
public class ConversationAnswerModular : IPracticeModular
{
    public override void Initial<T>(T content)
    {
        // 初始化對話回答練習
        // 準備對話內容
        // 設置評分標準
    }
    
    public override void Run()
    {
        // 顯示對話情境
        // 播放對話前半部分
        // 等待用戶回答
    }
    
    public override void Stop()
    {
        // 評估回答結果
        // 生成結果
    }
}
```

對話回答練習顯示一個對話情境，用戶需要根據情境回答問題或完成對話。主要特點包括：

- **情境理解**：訓練用戶在特定情境中的語言應用
- **開放回答**：允許多種可能的正確回答
- **角色扮演**：用戶扮演對話中的一個角色

## 練習類型配置

每種練習類型都可以通過 `ModularConfig` 進行配置，以適應不同的學習需求和難度級別：

```csharp
public class ModularConfig
{
    public PracticeType practiceType;      // 練習類型
    public LevelDiff levelDiff;            // 難度級別
    public bool showHint;                  // 是否顯示提示
    public bool autoPlay;                  // 是否自動播放
    public float timeLimit;                // 時間限制
    public int maxAttempts;                // 最大嘗試次數
    public float passingScore;             // 及格分數
    public Dictionary<string, object> properties; // 其他屬性
}
```

通過調整這些配置參數，可以為不同的用戶和學習階段提供適當的挑戰和支持。

## 練習類型擴展

LearningPal 系統的設計允許輕鬆添加新的練習類型。要添加新的練習類型，需要：

1. 在 `PracticeType` 枚舉中添加新類型
2. 創建實現 `IPracticeModular` 接口的新類
3. 實現必要的方法和邏輯
4. 在練習流程中註冊新類型

例如，添加一個新的「句子重組」練習類型：

```csharp
// 1. 在 PracticeType 枚舉中添加新類型
public enum PracticeType
{
    // 現有類型...
    SentenceRearrangement,                 // 句子重組
    // 其他新類型...
}

// 2. 創建新的練習模組類
public class SentenceRearrangementModular : IPracticeModular
{
    // 3. 實現必要的方法和邏輯
    public override void Initial<T>(T content)
    {
        // 初始化邏輯
    }
    
    public override void Run()
    {
        // 運行邏輯
    }
    
    public override void Stop()
    {
        // 停止邏輯
    }
}

// 4. 在練習流程中註冊新類型
public class PracticeModularFactory
{
    public static IPracticeModular CreateModular(PracticeType practiceType)
    {
        switch (practiceType)
        {
            // 現有類型...
            case PracticeType.SentenceRearrangement:
                return new SentenceRearrangementModular();
            // 其他類型...
            default:
                return null;
        }
    }
}
```

通過這種方式，系統可以不斷擴展以支持新的練習類型和學習方法。

---

本文檔提供了 LearningPal 系統支持的練習類型的概述。這些練習類型共同構成了系統的教學功能，支持多樣化的語言學習體驗。
