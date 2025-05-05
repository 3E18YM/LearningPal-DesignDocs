---
title: 設計系統經歷與說明
slug: /overview
---

# 設計系統經歷與說明 (LearningPal Design Overview)

## 對象
本說明文件適用於：
- 前端開發者（Unity）
- 後端維運工程師（Firebase、API）
- 教師與教材設計者（管理平台）
- 內容編輯者（單字/例句設計）

---

## 平台構想

### 1. 前端APP
- 使用 Unity 開發 iOS / Android App
- 採 UI Toolkit (UXML / USS) 構建主組件
- 組合 Zenject / UniTask 處理相依性和非同步操作
- 使用 Firebase Auth / Firestore 執行資料操作 / 登入 / Storage

### 2. 後端網站
#### 用戶端
- 為學生與家長提供學習進度追蹤與統計
- 顯示訓練結果與評分
- 未來採用 AI 分析調整電子訓練計劃

#### 管理端
- 教師可控制單字、例句、對話、圖片
- 合併 Pixabay API 快速新增學習資源
- 自訂學習流程 (訓練次數、時間限制、經驗值取得...)

---

## 單字資料組織模型 (Firebase JSON Format)

與 Firestore 谷欄組織相對應

```json
{
  "vocab_id": "v_1",
  "word": "book",
  "phonetic": "bʊk",
  "translations": {
    "zh": "書",
    "vi": "Sách"
  },
  "tags": ["Basic"],
  "examples": [
    {
      "sentence_id": "s_1",
      "text": "Where's my book?",
      "translation": {
        "zh": "我的書在哪?",
        "vi": "Sách của tôi ở đâu?"
      }
    }
  ]
}
```

> ✅ 每筆字卡支援多語翻譯（繁體中文、越南語），並包含例句與翻譯

---

## Unity 經歷實作模組

### 展示模組類型

- `MultipleChoice` 選擇題
- `TrueOrFalse` 是非題
- `PuzzleMatch` 拼圖配對
- `KeyboardSpelling` 鍵盤拼字
- `Pronunciation` 發音評分（Azure Speech SDK）

### 組件 JSON 配置組合
- `cardLayoutConfig`: UI 配置
- `modularConfig`: 模組类型與表現功能
- `content`: 題目內容 (word, image, audio)
- `properties`: 題目配置 (正確答案、評分方式...)

---

## Firebase 內容管理功能

- `Firestore`: 儲存單字、經驗值、成績
- `Firebase Auth`: 學生登入與身份驗證
- `Firebase Storage`: 儲存聲音、圖片等檔案
- `Cloud Functions`: 展望針對操作輸出觀察算法

---

## 未來預展
- AI 分析學習執行負責分配與次數設計
- 讓教師管理更多單元化的關卡與練習流程
- 圖像、聲音、書面內容最終將實現 DLC 形式充分汽化

---

## 連結
- GitHub Repo: [LearningPal-DesignDocs](https://github.com/LearningPal-DesignDocs)
- Firebase Console: (access needed)
- Unity Asset Repo: (private access planned)

---

任何資料或添加項目請聯絡經理人：`@LearningPalTeam`
