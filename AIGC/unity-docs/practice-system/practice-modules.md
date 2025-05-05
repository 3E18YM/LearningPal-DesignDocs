---
title: 練習模組 Practice Modules
sidebar_position: 3
---

# 練習模組

LearningPal 系統的練習模組實現了各種練習類型的具體功能。本文檔詳細介紹了練習模組的設計和實現。

## 練習模組概述

練習模組是 LearningPal 系統的核心組件之一，負責：

1. **內容呈現**：顯示練習內容和提示
2. **用戶交互**：處理用戶輸入和操作
3. **答案評估**：評估用戶回答的正確性
4. **反饋生成**：提供即時視覺和聲音反饋
5. **結果記錄**：記錄用戶表現和評分

練習模組通過 `IPracticeModular` 抽象類定義，不同類型的練習模組實現這個抽象類以提供特定的功能。

## 基本練習模組

`IPracticeModular` 抽象類定義了所有練習模組的基本行為：

```csharp
public abstract class IPracticeModular : MonoBehaviour
{
    public ModularConfig Config;           // 模組配置
    public List<TextButtonHandler> textButtonHandlers; // 文本按鈕處理器
    public ModularResult result;           // 練習結果
    internal bool hint = false;            // 提示標記
    public bool isComplete = false;        // 完成標記
    public abstract void Initial<T>(T content); // 初始化方法
    public virtual void Hint() { hint = true; } // 提示方法
    public virtual void Run() { }          // 運行方法
    public virtual void Stop() { }         // 停止方法
    public UnityEvent<IPracticeModular> OnAnswer; // 回答事件
}
```

這個抽象類定義了練習模組的生命週期和基本功能，是所有練習模組的基礎。

## 多選題模組

`MultipleChoiceModular` 類實現了多選題練習：

```csharp
public class MultipleChoiceModular : IPracticeModular
{
    public List<Button> optionButtons;     // 選項按鈕
    public Text questionText;              // 問題文本
    public Image questionImage;            // 問題圖片
    
    private string correctAnswer;          // 正確答案
    private List<string> options;          // 選項列表
    
    public override void Initial<T>(T content)
    {
        // 初始化多選題練習
        // 設置問題和選項
        // 隨機排列選項
        // 設置按鈕事件
    }
    
    public override void Run()
    {
        // 顯示問題和選項
        // 啟用按鈕交互
    }
    
    public override void Stop()
    {
        // 禁用按鈕交互
        // 清理資源
    }
    
    private void OnOptionSelected(int index)
    {
        // 處理選項選擇
        // 評估答案正確性
        // 更新結果
        // 提供反饋
        // 觸發回答事件
    }
    
    public override void Hint()
    {
        // 提供提示
        // 可能禁用部分錯誤選項
        // 或突出顯示正確選項
        
        hint = true;
    }
}
```

這個類實現了多選題練習的特定功能，包括選項顯示、選擇處理和答案評估。

## 拼寫練習模組

`SpellingWordModular` 類實現了單詞拼寫練習：

```csharp
public class SpellingWordModular : IPracticeModular
{
    public Transform letterButtonContainer; // 字母按鈕容器
    public Transform answerContainer;      // 答案容器
    public Button hintButton;              // 提示按鈕
    public Button clearButton;             // 清除按鈕
    public Button submitButton;            // 提交按鈕
    
    private string correctAnswer;          // 正確答案
    private List<Button> letterButtons;    // 字母按鈕列表
    private List<Text> answerTexts;        // 答案文本列表
    private string currentAnswer = "";     // 當前答案
    
    public override void Initial<T>(T content)
    {
        // 初始化拼寫練習
        // 設置正確答案
        // 生成字母按鈕
        // 設置按鈕事件
    }
    
    public override void Run()
    {
        // 顯示拼寫界面
        // 啟用按鈕交互
    }
    
    public override void Stop()
    {
        // 禁用按鈕交互
        // 清理資源
    }
    
    private void OnLetterSelected(string letter)
    {
        // 處理字母選擇
        // 更新當前答案
        // 更新答案顯示
    }
    
    private void OnClearButtonClicked()
    {
        // 清除當前答案
        // 重置答案顯示
    }
    
    private void OnSubmitButtonClicked()
    {
        // 提交當前答案
        // 評估答案正確性
        // 更新結果
        // 提供反饋
        // 觸發回答事件
    }
    
    public override void Hint()
    {
        // 提供提示
        // 可能顯示部分正確字母
        // 或禁用部分錯誤字母
        
        hint = true;
    }
}
```

這個類實現了拼寫練習的特定功能，包括字母選擇、答案組合和提交評估。

## 語音練習模組

`ReadItOutModular` 類實現了朗讀練習：

```csharp
public class ReadItOutModular : IPracticeModular
{
    public Button recordButton;            // 錄音按鈕
    public Image recordIndicator;          // 錄音指示器
    public Text promptText;                // 提示文本
    public AudioSource audioSource;        // 音頻源
    
    private string correctText;            // 正確文本
    private bool isRecording = false;      // 是否正在錄音
    private AudioClip recordedClip;        // 錄製的音頻
    
    public override void Initial<T>(T content)
    {
        // 初始化朗讀練習
        // 設置正確文本
        // 設置按鈕事件
        // 初始化語音識別服務
    }
    
    public override void Run()
    {
        // 顯示朗讀界面
        // 啟用按鈕交互
    }
    
    public override void Stop()
    {
        // 停止錄音
        // 禁用按鈕交互
        // 清理資源
    }
    
    private void OnRecordButtonClicked()
    {
        // 切換錄音狀態
        if (!isRecording)
        {
            // 開始錄音
            StartRecording();
        }
        else
        {
            // 停止錄音
            StopRecording();
            // 評估錄音
            EvaluateRecording();
        }
    }
    
    private void StartRecording()
    {
        // 開始錄音
        // 更新 UI 狀態
        isRecording = true;
    }
    
    private void StopRecording()
    {
        // 停止錄音
        // 更新 UI 狀態
        isRecording = false;
    }
    
    private async void EvaluateRecording()
    {
        // 將錄音發送到語音識別服務
        // 獲取識別結果
        // 評估與正確文本的匹配度
        // 更新結果
        // 提供反饋
        // 觸發回答事件
    }
    
    public override void Hint()
    {
        // 提供提示
        // 可能播放標準發音
        // 或突出顯示關鍵詞
        
        hint = true;
    }
}
```

這個類實現了朗讀練習的特定功能，包括錄音控制、語音識別和發音評估。

## 聽力練習模組

`ListeningModular` 類實現了聽力練習：

```csharp
public class ListeningModular : IPracticeModular
{
    public Button playButton;              // 播放按鈕
    public List<Button> optionButtons;     // 選項按鈕
    public Slider progressSlider;          // 進度滑塊
    public Text questionText;              // 問題文本
    
    private AudioClip audioClip;           // 音頻片段
    private string correctAnswer;          // 正確答案
    private List<string> options;          // 選項列表
    
    public override void Initial<T>(T content)
    {
        // 初始化聽力練習
        // 加載音頻資源
        // 設置問題和選項
        // 設置按鈕事件
    }
    
    public override void Run()
    {
        // 顯示聽力界面
        // 啟用按鈕交互
    }
    
    public override void Stop()
    {
        // 停止音頻播放
        // 禁用按鈕交互
        // 清理資源
    }
    
    private void OnPlayButtonClicked()
    {
        // 播放音頻
        // 更新 UI 狀態
    }
    
    private void OnOptionSelected(int index)
    {
        // 處理選項選擇
        // 評估答案正確性
        // 更新結果
        // 提供反饋
        // 觸發回答事件
    }
    
    public override void Hint()
    {
        // 提供提示
        // 可能重複播放音頻
        // 或顯示部分文本
        
        hint = true;
    }
}
```

這個類實現了聽力練習的特定功能，包括音頻播放、問題顯示和答案評估。

## 對話練習模組

`ConversationAnswerModular` 類實現了對話回答練習：

```csharp
public class ConversationAnswerModular : IPracticeModular
{
    public Text conversationText;          // 對話文本
    public Button recordButton;            // 錄音按鈕
    public Image recordIndicator;          // 錄音指示器
    public Text promptText;                // 提示文本
    
    private Conversation conversation;     // 對話內容
    private int currentSentenceIndex;      // 當前句子索引
    private bool isRecording = false;      // 是否正在錄音
    
    public override void Initial<T>(T content)
    {
        // 初始化對話練習
        // 設置對話內容
        // 設置按鈕事件
        // 初始化語音識別服務
    }
    
    public override void Run()
    {
        // 顯示對話界面
        // 播放對話前半部分
        // 啟用按鈕交互
    }
    
    public override void Stop()
    {
        // 停止錄音
        // 禁用按鈕交互
        // 清理資源
    }
    
    private void OnRecordButtonClicked()
    {
        // 切換錄音狀態
        if (!isRecording)
        {
            // 開始錄音
            StartRecording();
        }
        else
        {
            // 停止錄音
            StopRecording();
            // 評估錄音
            EvaluateRecording();
        }
    }
    
    private void StartRecording()
    {
        // 開始錄音
        // 更新 UI 狀態
        isRecording = true;
    }
    
    private void StopRecording()
    {
        // 停止錄音
        // 更新 UI 狀態
        isRecording = false;
    }
    
    private async void EvaluateRecording()
    {
        // 將錄音發送到語音識別服務
        // 獲取識別結果
        // 評估與正確回答的匹配度
        // 更新結果
        // 提供反饋
        // 觸發回答事件
    }
    
    public override void Hint()
    {
        // 提供提示
        // 可能顯示正確回答
        // 或播放標準發音
        
        hint = true;
    }
}
```

這個類實現了對話練習的特定功能，包括對話顯示、錄音控制和回答評估。

## 練習模組配置

練習模組通過 `ModularConfig` 類進行配置：

```csharp
[System.Serializable]
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

通過調整這些配置參數，可以為不同的學習場景和用戶需求創建適當的練習體驗。

## 練習模組結果

練習模組使用 `ModularResult` 類記錄練習結果：

```csharp
[System.Serializable]
public class ModularResult
{
    public string data;                    // 答案數據
    public CompletionStatus status;        // 完成狀態
    public bool hint;                      // 是否使用提示
    public float score;                    // 分數
}
```

這個類包含了練習的結果信息，包括用戶的答案、完成狀態、是否使用提示和分數。

## 練習模組工廠

`PracticeModularFactory` 類負責創建練習模組：

```csharp
public class PracticeModularFactory
{
    public static IPracticeModular CreateModular(PracticeType practiceType)
    {
        switch (practiceType)
        {
            case PracticeType.MultipleChoice:
                return new MultipleChoiceModular();
            case PracticeType.SpellingWord:
                return new SpellingWordModular();
            case PracticeType.ReadItOut:
                return new ReadItOutModular();
            case PracticeType.Listening:
                return new ListeningModular();
            case PracticeType.ConversationAnswer:
                return new ConversationAnswerModular();
            // 其他類型...
            default:
                return null;
        }
    }
}
```

這個工廠類根據練習類型創建相應的練習模組，便於系統動態選擇和使用不同的練習類型。

## 練習模組與卡片交互

練習模組通常與練習卡片結合使用：

```csharp
public class VocabularyCard : IPracticeCard<Vocabulary>
{
    public override void Initial(Vocabulary content, CardLayoutConfig cardConfig, ModularConfig modularConfig)
    {
        // 設置卡片內容和配置
        this.content = content;
        this.cardConfig = cardConfig;
        this.modularConfig = modularConfig;
        
        // 創建練習模組
        practiceModular = PracticeModularFactory.CreateModular(modularConfig.practiceType);
        
        // 初始化練習模組
        practiceModular.Initial(content);
        
        // 註冊事件
        practiceModular.OnAnswer.AddListener(OnModularAnswer);
        
        // 觸發開始事件
        OnStartPractice?.Invoke(this);
    }
    
    private void OnModularAnswer(IPracticeModular modular)
    {
        // 處理模組回答
        // 更新練習結果
        practiceResult = new PracticeResult
        {
            guid = Guid.NewGuid().ToString(),
            contentType = ContentType.Vocabulary,
            contentID = content.ID.ToString(),
            dateTime = DateTime.Now,
            score = modular.result.score,
            status = modular.result.status,
            modularResult = new List<ModularResult> { modular.result }
        };
        
        // 觸發練習完成事件
        OnPracticed?.Invoke(this);
        
        // 如果完成，觸發結束事件
        if (modular.isComplete)
        {
            OnEndPractice?.Invoke(this);
        }
    }
}
```

這種設計使得卡片可以包含不同類型的練習模組，提供多樣化的學習體驗。

## 練習模組擴展

LearningPal 系統的設計允許輕鬆添加新的練習模組。要添加新的練習模組，需要：

1. 創建實現 `IPracticeModular` 抽象類的新類
2. 實現必要的方法和邏輯
3. 在練習模組工廠中註冊新類型

例如，添加一個新的「句子重組」練習模組：

```csharp
// 1. 創建新的練習模組類
public class SentenceRearrangementModular : IPracticeModular
{
    public Transform wordContainer;        // 單詞容器
    public Transform answerContainer;      // 答案容器
    public Button submitButton;            // 提交按鈕
    
    private string correctSentence;        // 正確句子
    private List<string> words;            // 單詞列表
    private List<Transform> wordObjects;   // 單詞對象列表
    
    // 2. 實現必要的方法和邏輯
    public override void Initial<T>(T content)
    {
        // 初始化句子重組練習
        // 設置正確句子
        // 分割句子為單詞
        // 隨機排列單詞
        // 創建單詞對象
        // 設置拖放事件
    }
    
    public override void Run()
    {
        // 顯示句子重組界面
        // 啟用拖放交互
    }
    
    public override void Stop()
    {
        // 禁用拖放交互
        // 清理資源
    }
    
    private void OnSubmitButtonClicked()
    {
        // 獲取當前句子
        // 評估與正確句子的匹配度
        // 更新結果
        // 提供反饋
        // 觸發回答事件
    }
    
    public override void Hint()
    {
        // 提供提示
        // 可能顯示部分正確單詞順序
        // 或突出顯示某些關鍵單詞
        
        hint = true;
    }
}

// 3. 在練習模組工廠中註冊新類型
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

## 練習模組與 UI 整合

練習模組通過 UI 預製體與用戶界面整合：

```csharp
public class ModularUIManager : MonoBehaviour
{
    public Dictionary<PracticeType, GameObject> modularPrefabs; // 模組預製體字典
    
    public IPracticeModular CreateModular(PracticeType practiceType, Transform parent)
    {
        // 檢查是否有對應的預製體
        if (!modularPrefabs.ContainsKey(practiceType))
        {
            Debug.LogError($"No prefab found for practice type: {practiceType}");
            return null;
        }
        
        // 實例化預製體
        GameObject modularObject = Instantiate(modularPrefabs[practiceType], parent);
        
        // 獲取練習模組組件
        IPracticeModular modular = modularObject.GetComponent<IPracticeModular>();
        
        return modular;
    }
}
```

這種設計使得系統可以動態創建和顯示不同類型的練習模組，提供一致的用戶體驗。

## 練習模組與多媒體整合

練習模組可以整合多種多媒體資源：

```csharp
public class MultimediaManager : MonoBehaviour
{
    public static MultimediaManager Instance; // 單例實例
    
    public Dictionary<string, AudioClip> audioClips; // 音頻片段字典
    public Dictionary<string, Sprite> images;       // 圖片字典
    
    private void Awake()
    {
        // 設置單例
        if (Instance == null)
        {
            Instance = this;
            DontDestroyOnLoad(gameObject);
        }
        else
        {
            Destroy(gameObject);
        }
    }
    
    public AudioClip GetAudio(string key)
    {
        // 獲取音頻資源
        if (audioClips.ContainsKey(key))
        {
            return audioClips[key];
        }
        
        // 嘗試動態加載
        return Resources.Load<AudioClip>($"Audio/{key}");
    }
    
    public Sprite GetImage(string key)
    {
        // 獲取圖片資源
        if (images.ContainsKey(key))
        {
            return images[key];
        }
        
        // 嘗試動態加載
        return Resources.Load<Sprite>($"Images/{key}");
    }
}
```

練習模組可以使用這個管理器加載和使用多媒體資源，豐富學習體驗。

---

本文檔提供了 LearningPal 系統練習模組的概述。練習模組是系統的核心組件，實現了各種練習類型的具體功能，支持多樣化的語言學習體驗。
