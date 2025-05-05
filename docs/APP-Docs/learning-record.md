---
title: 學習歷程 Learning Journey
---

# 學習歷程系統

學習歷程系統是 LearningPal 應用程式中追蹤和顯示用戶學習進度的核心功能。本文檔詳細說明了學習歷程的數據結構、記錄機制和顯示方式。

## 系統概述

學習歷程系統主要由以下幾個部分組成：

1. **結果數據結構**：定義了如何存儲練習結果的數據模型
2. **評分機制**：計算用戶表現的評分系統
3. **歷程顯示**：展示用戶過去學習記錄的界面

學習歷程系統在 LearningPal 應用中扮演著關鍵角色，它不僅記錄用戶的學習活動，還提供了激勵機制，通過可視化的進度展示和成就系統，鼓勵用戶持續學習。

## 數據結構

### FlowResult 類

`FlowResult` 類是記錄整個學習流程結果的主要數據結構：

```csharp
public class FlowResult
{
    public Guid guid;                      // 唯一識別碼
    public DateTime dateTime;              // 完成時間
    public string levelName;               // 關卡或測驗名稱
    public string levelID;                 // 關卡或測驗ID
    public int rating;                     // 星級評分 (1-3星)
    public int completion;                 // 完成度
    public int energyScore;                // 能量分數
    public int achievementScore;           // 成就分數
    public TimeSpan timeSpan;              // 完成時間
    public List<PracticeResult> practiceResults; // 各練習結果列表
}
```

每個 `FlowResult` 實例代表用戶完成的一個學習流程（如一個關卡或一次測驗），包含了該流程的所有相關信息，如完成時間、評分、以及包含的所有練習結果。

### PracticeResult 類

`PracticeResult` 類記錄單個練習項目的結果：

```csharp
public class PracticeResult
{
    public string guid;                    // 唯一識別碼
    public string levelID;                 // 關卡ID
    public ContentType contentType;        // 內容類型 (詞彙、句子、對話)
    public string contentID;               // 內容ID
    public CommendRating commendRating;    // 評級 (失敗、良好、完美、優秀)
    public DateTime dateTime;              // 完成時間
    public TimeSpan timeSpan;              // 練習時間
    public CompletionStatus status;        // 完成狀態
    public float score;                    // 分數
    public int attemptCount;               // 嘗試次數
    public ModularConfig modularConfig;    // 模組配置
    public List<ModularResult> modularResult; // 模組結果列表
}
```

`PracticeResult` 記錄了用戶在單個練習項目（如一個詞彙卡片或一個句子練習）中的表現，包括評級、分數、嘗試次數等信息。

### ModularResult 類

`ModularResult` 類記錄每次答題的詳細結果：

```csharp
public class ModularResult
{
    public string data;                    // 答案數據
    public CompletionStatus status;        // 完成狀態
    public bool hint;                      // 是否使用提示
    public float score;                    // 分數
}
```

`ModularResult` 是最細粒度的結果記錄，它記錄了用戶在每次答題嘗試中的具體表現，包括提交的答案、是否使用了提示、以及獲得的分數。

## 評分機制

### 評級系統

系統使用多種評級機制來評估用戶表現：

```csharp
public enum CommendRating
{
    Null,                                  // 未評級
    Failed,                                // 失敗
    Good,                                  // 良好
    Perfect,                               // 完美
    Excellent                              // 優秀
}

public enum CompletionStatus
{
    Null,                                  // 未完成
    Correct,                               // 正確
    Fail,                                  // 失敗
    Skip,                                  // 跳過
    Completed,                             // 已完成
    Uncompleted,                           // 未完成
    Timeout,                               // 超時
    T,                                     // 真 (用於真假題)
    F                                      // 假 (用於真假題)
}
```

這些枚舉類型定義了系統中使用的各種評級和狀態，用於精確描述用戶的學習表現。

### 星級評分

系統根據用戶在關卡中的表現計算星級評分（1-3星）：

```csharp
public static int BaseStarRating(this FlowResult flowResult, LevelDiff levelDiff)
{
    int enumLength = Enum.GetValues(typeof(CommendRating)).Length;
    List<int> Total = Enumerable.Repeat(0, enumLength).ToList();

    foreach (var result in flowResult.practiceResults)
    {
        //處理未評比關卡
        if (result.commendRating == CommendRating.Null)
        {
            result.commendRating = result.ToCommendRating(levelDiff);
        }

        Total[(int)result.commendRating] += 1;
    }
    
    int LevelCount = flowResult.practiceResults.Count;

    if (Total[1] == 0) // 沒有失敗的練習
    {
        int rating = Total[4] >= LevelCount * 0.5 ? 3 :   //最高通關標準: Excellent50%以上
                    Total[3] + Total[4] >= LevelCount * 0.75 ? 2 : //中間通關標準: Perfect75%以上
                    1; //最低通關標準: Good100%以上          
        return rating;
    }
    return 0; // 有失敗的練習，不給星
}
```

不同類型的練習有不同的評分標準：

- **詞彙和句子練習**：
  - 全部答對獲得1星
  - 75%以上達到「完美」評級獲得2星
  - 50%以上達到「優秀」評級獲得3星

- **對話練習**：
  - 通過標記關卡獲得1星
  - 通過標記關卡和一半其他關卡獲得2星
  - 全部通過獲得3星

### 能量分數計算

能量分數根據完成的練習數量和嘗試次數計算：

```csharp
public static int CalculateEnergyScore(this FlowResult flowResult)
{
    var practiceScore = flowResult.practiceResults.Count * 10;
    var pronScore = flowResult.practiceResults.Sum(x => x.modularResult.Count);
    var total = practiceScore + pronScore;
    return total;
}
```

能量分數是激勵用戶的重要機制，它反映了用戶的學習投入度，計算方式為：
- 每完成一個練習項目獲得10點基礎分數
- 每次答題嘗試額外獲得1點分數

## 學習歷程顯示

### LearningJourneyHandler 類

`LearningJourneyHandler` 類管理學習歷程的顯示：

```csharp
public class LearningJourneyHandler : MonoBehaviour
{
    [SerializeField] private LearningResultPanel VocabularyResult;
    [SerializeField] private LearningResultPanel ConversationResult;
    [SerializeField] private LearningResultPanel SpellingBeeResult;
    [SerializeField] private Transform container;
    [SerializeField] private Scrollbar scrollbar;
    
    private List<FlowResult> levelResults;
    int count = 0;
    bool isLoading = false;
    
    // 初始化學習歷程面板，顯示最近的結果
    async void Initial();
    
    // 為每個完成的流程創建結果面板
    async Task CreatePanel(FlowResult flowResult);
    
    // 當用戶滾動時加載更多面板
    async void LoadPanel();
}
```

`LearningJourneyHandler` 負責管理學習歷程界面，它從 `GameManager.dataStorage.levelResults` 獲取用戶的學習記錄，並將其顯示在界面上。

### 面板類型

系統根據內容類型顯示不同的結果面板：

- **VocabularyResult**：詞彙練習結果面板，顯示詞彙學習的相關信息
- **ConversationResult**：對話練習結果面板，顯示對話練習的相關信息
- **SpellingBeeResult**：拼寫練習結果面板，顯示拼寫練習的相關信息

每種面板都針對特定類型的練習進行了優化，以最適合的方式展示該類型練習的結果。

### 動態加載

學習歷程界面實現了動態加載機制，當用戶滾動到底部時自動加載更多歷史記錄：

```csharp
void Update()
{
    if (isLoading == false)
    {
        if (scrollbar.value < 0.5 / count && count < levelResults.Count)
        {
            Debug.Log("Create New Panel");
            LoadPanel();
        }
    }
}
```

這種機制確保了界面的流暢性，即使用戶有大量的學習記錄，也不會一次性加載所有內容，而是根據需要逐步加載。

## 數據持久化

學習歷程數據通過以下方式保存：

1. **本地存儲**：結果保存在 `GameManager.dataStorage.levelResults` 中，這是一個內存中的列表，在應用關閉前會被序列化到本地存儲。

2. **服務器同步**：通過 `GameManager.server.UpdateData(levelResult)` 將結果同步到服務器，確保用戶在不同設備上也能看到一致的學習記錄。

3. **用戶數據保存**：通過 `GameManager.server.saveUserData()` 保存用戶數據，包括學習進度、成就等信息。

```csharp
private void SaveLevelData(FlowResult levelResult)
{
    //保存關卡通關資訊到全域變數LastLevelDatas
    levelResult.energyScore = levelResult.CalculateEnergyScore();
    levelResult.completion = levelResult.CalculateCompletion(practiceLevel);
    var levelController = GameManager.sceneController.levelLoader;

    GameManager.dataStorage.LevelDatas.UpdateScoreByLevelID(new PracticeLevel(
        levelResult.levelID,
        levelResult.levelName,
        rating: levelResult.rating,
        completion: levelResult.completion
        ));
    GameManager.dataStorage.LevelDatas.SaveData();// 保存過關資料      
    GameManager.dataStorage.levelResults.Insert(0, levelResult);//保存完整關卡資料
    GameManager.server.UpdateData(levelResult);
    GameManager.server.saveUserData();
}
```

## 結果展示界面

### LevelResultPanel 類

`LevelResultPanel` 類顯示關卡完成後的結果：

```csharp
public class LevelResultPanel : MonoBehaviour
{
    [Inject] IPracticeFlow practiceFlow;
    [SerializeField] private PracticeLevel practiceLevel;
    [SerializeField] private PracticeResultPreview VocabularyResultPreview;
    [SerializeField] private PracticeResultPreview ConversationResultPreview;
    [SerializeField] private LevelButtonsHandler levelButtonsHandler;
    [SerializeField] private StartRating startRating;
    public EnergyBar energyBar;
    [SerializeField] private Transform practiceResultsContainer;
    [SerializeField] private Animator animator;
    
    // 初始化結果面板
    public async Task Initial(FlowResult levelResult);
    
    // 顯示個別練習結果
    async Task ShowResults(List<PracticeResult> results);
    
    // 動畫顯示星級評分
    async Task Rating(FlowResult flowResult);
    
    // 創建練習結果卡片
    async Task<PracticeResultPreview> CreateResultCards(PracticeResult practiceResult);
    
    // 關閉面板
    public async Task ClosePanel();
}
```

結果面板包含以下主要元素：

1. **星級評分**：通過 `startRating` 組件顯示用戶獲得的星星數量，使用動畫效果增強視覺體驗。

2. **能量條**：通過 `energyBar` 組件顯示獲得的能量分數，能量是用戶在系統中的一種虛擬貨幣，可用於解鎖新內容或獲取獎勵。

3. **練習結果列表**：在 `practiceResultsContainer` 中顯示每個練習項目的詳細結果，包括評級、分數、用時等信息。

4. **按鈕控制**：通過 `levelButtonsHandler` 提供重試、繼續等操作選項，讓用戶可以根據自己的需求選擇下一步操作。

結果展示界面不僅提供了學習成果的反饋，還通過視覺化的方式激勵用戶，增強學習動力。

---

本文檔提供了 LearningPal 學習歷程系統的概述。如需更詳細的實現細節，請參考源代碼或聯繫開發團隊。
