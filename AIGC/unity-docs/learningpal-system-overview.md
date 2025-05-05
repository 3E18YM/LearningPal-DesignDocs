# LearningPal System Overview

LearningPal is a Unity-based educational application designed to facilitate language learning through interactive exercises, vocabulary practice, and conversation simulations. This document provides a comprehensive overview of the system architecture, components, and functionality.

## Table of Contents

- [System Architecture](#system-architecture)
- [Core Components](#core-components)
- [Practice System](#practice-system)
- [Content Management](#content-management)
- [User Progress Tracking](#user-progress-tracking)
- [UI Flow](#ui-flow)

## System Architecture

LearningPal follows an object-oriented architecture with a focus on modularity and extensibility. The system is built around several key interfaces and abstract classes that define the behavior of various components.

### Key Architectural Patterns

- **Interface-based Design**: Core functionality is defined through interfaces like `ICard`, `IPracticeFlow`, and `IPracticeModular`.
- **Dependency Injection**: The system uses Zenject for dependency injection to manage component dependencies.
- **Event-driven Communication**: Components communicate through Unity Events to maintain loose coupling.
- **Modular Practice System**: Different types of exercises are implemented as modular components that can be combined to create varied learning experiences.

### Technology Stack

- **Unity Engine**: Core game engine
- **C#**: Primary programming language
- **DOTween**: Animation library for UI transitions
- **Zenject**: Dependency injection framework
- **Azure Speech Services**: For pronunciation assessment and speech recognition

## Core Components

### Interfaces and Base Classes

#### ICard Interface
Defines the basic behavior for card-based UI elements:
```csharp
public interface ICard
{
    void Reset();
    void SpawnAnim(float duration = 0.5f, Ease ease = Ease.OutQuart);
    void Scale(float scale);
}
```

#### IPracticeFlow
Abstract base class for practice flow management:
```csharp
public abstract class IPracticeFlow : MonoBehaviour
{
    public PracticeType practiceType;
    public abstract void Initial();
    public abstract Task StartFlow();
    public abstract Task Stop();
    public abstract Task Pause();
    public abstract void ForceStop();
    public abstract void UpdateResult();
    // Events for flow state changes
    public UnityEvent<IPracticeFlow> OnStartedFlow;
    public UnityEvent<IPracticeFlow> OnFlow;
    public UnityEvent<IPracticeFlow> OnEndFlow;
}
```

#### IPracticeModular
Abstract base class for practice modules:
```csharp
public abstract class IPracticeModular : MonoBehaviour
{
    public ModularConfig Config;
    public ModularResult result;
    public abstract void Initial<T>(T content);
    public virtual void Hint() { hint = true; }
    public virtual void Run() { }
    public virtual void Stop() { }
    public UnityEvent<IPracticeModular> OnAnswer;
}
```

### Data Structures

#### EducationContent
Contains collections of vocabulary and conversations:
```csharp
public class EducationContent
{
    public Vocabularies Vocabularies;
    public Conversations Conversations;
}
```

#### PracticeLevel
Represents a learning level with associated quests:
```csharp
public class PracticeLevel
{
    public string ID;
    public string levelName;
    public PracticeType practiceType;
    public LevelDiff level;
    public ContentType contentType;
    public List<PracticeQuest> practiceQuests;
    public Dictionary<string, object> properties;
}
```

#### FlowResult
Stores the results of a completed practice flow:
```csharp
public class FlowResult
{
    public Guid guid;
    public DateTime dateTime;
    public string levelName;
    public string levelID;
    public int rating;
    public int completion;
    public int energyScore;
    public int achievementScore;
    public TimeSpan timeSpan;
    public List<PracticeResult> practiceResults;
}
```

## Practice System

The practice system is the core of LearningPal, providing various exercise types to help users learn language skills.

### Practice Types

LearningPal supports multiple practice types defined in the `PracticeType` enum:

```csharp
public enum PracticeType
{
    SeeAndSay,
    ReadItOut,
    ReadSentence,
    Listening,
    SeeAndSpelling,
    SpellingWord,
    SentenceSpelling,
    PuzzleSpelling,
    PuzzleSentence,
    MultipleChoice,
    SeeAndChoose,
    ChooseItRight,
    SentenceChoose,
    TrueOrFalse,
    ConversationRepeat,
    ConversationAnswer,
    Custom
}
```

### Practice Modules

#### MultipleChoiceModular
Implements multiple-choice questions where users select from provided options:

```csharp
public class MultipleChoiceModular : IPracticeModular
{
    // Creates option buttons for users to select
    public async Task CreateOption(string text, float duration = 0.5f, Ease ease = Ease.OutQuart);
    
    // Handles button click events and evaluates answers
    public void Button_OnClick(Button button);
}
```

#### PronunciationModular
Evaluates user pronunciation using speech recognition:

```csharp
public class PronunciationModular : IPracticeModular
{
    // Analyzes user pronunciation against a reference text
    public async Task PronunciationAnalysis(string referenceText, string languageCode = null, float autoStop = -1, bool pauseReset = false);
    
    // Displays pronunciation scores on UI elements
    public async Task DisplayPronunciationScores(List<TextButton_UI> textButtons);
}
```

#### KeyboardSpellingModular
Implements spelling exercises where users type answers:

```csharp
public class KeyboardSpellingModular : IPracticeModular
{
    // Handles keyboard input for spelling exercises
    public async Task KeyBoardInputListener();
    
    // Processes user input and validates against correct answers
    public void inputHandler(KeyBoardButton keyBoardButton, string key);
}
```

### Practice Flow

The practice flow manages the sequence of practice cards and exercises:

```csharp
public abstract class IPracticeCardFlow<T> : IPracticeFlow
{
    public int cardState = 0; // Current card index
    public int StateCount = 0; // Total number of cards
    public IPracticeCardGenerator<T> Generator; // Card generator
}
```

## Content Management

LearningPal organizes educational content into vocabularies, sentences, and conversations.

### Content Types

```csharp
public enum ContentType
{
    Null,
    Vocabulary,
    Sentence,
    Conversation
}
```

### Content Structure

#### Vocabulary
```csharp
public class Vocabulary
{
    public int ID;
    public string Word;
    public string Translation;
    public string Phonetic;
    public string Description;
    public Sentence Sentence;
    public string ImageUrl;
}
```

#### Conversation
```csharp
public class Conversation
{
    public string Title;
    public string Description;
    public List<string> Names;
    public List<ConversationSentence> Sentences;
    public string ImageUrl;
}
```

#### ConversationSentence
```csharp
public class ConversationSentence
{
    public int ID;
    public string ConversationID;
    public int OrderIndex;
    public string Name;
    public string Content;
    public string Translation;
}
```

### Content Collections

Content is organized into collections with different difficulty levels:

```csharp
public class Collection<T>
{
    public List<T> Basic;
    public List<T> Extend;
    public List<T> Review;
}
```

## User Progress Tracking

LearningPal tracks user progress through various result classes and scoring mechanisms.

### Result Tracking

#### PracticeResult
Tracks results for individual practice cards:

```csharp
public class PracticeResult
{
    public string guid;
    public string levelID;
    public ContentType contentType;
    public string contentID;
    public CommendRating commendRating;
    public DateTime dateTime;
    public TimeSpan timeSpan;
    public CompletionStatus status;
    public float score;
    public int attemptCount;
    public ModularConfig modularConfig;
    public List<ModularResult> modularResult;
}
```

#### ModularResult
Tracks individual answer attempts:

```csharp
public class ModularResult
{
    public string data;
    public CompletionStatus status;
    public bool hint;
    public float score;
}
```

### Rating System

The system uses several rating mechanisms:

```csharp
public enum CommendRating
{
    Null,
    Failed,
    Good,
    Perfect,
    Excellent
}

public enum CompletionStatus
{
    Null,
    Correct,
    Fail,
    Skip,
    Completed,
    Uncompleted,
    Timeout,
    T,
    F
}
```

### Learning Journey

The `LearningJourneyHandler` manages the display of learning history:

```csharp
public class LearningJourneyHandler : MonoBehaviour
{
    // Initializes the learning journey panel with recent results
    async void Initial();
    
    // Creates result panels for each completed flow
    async Task CreatePanel(FlowResult flowResult);
    
    // Loads additional panels as the user scrolls
    async void LoadPanel();
}
```

## UI Flow

### Main Menu

The main menu uses a card wheel navigation system:

```csharp
public class CardsWheel : MonoBehaviour
{
    private HorizontalScrollSnap horizontalScrollSnap;
    public MenuCard lastSelect;
    public ScrollRect scrollRect;
}
```

### Course Menu

The course menu displays available courses and levels:

```csharp
public class CourseCard : MonoBehaviour
{
    public EduResource eduResource;
    public ContentType contentType;
    
    // Shows level selection UI
    void ShowLevel(float duration = 0.5f, Ease ease = Ease.OutExpo);
    
    // Creates vocabulary level UI
    private async void CreateVocabularyLevels();
    
    // Creates conversation level UI
    private async void CreateConversationLevels();
}
```

### Conversation UI

Conversation exercises display dialog exchanges:

```csharp
public class ConversationLevelDetail : MonoBehaviour
{
    // Initializes the conversation UI with conversation data
    public async Task Initial(Conversation conversation);
    
    // Creates reply boxes for each sentence in the conversation
    private async Task CreateReply(Conversation conversation, int Index);
    
    // Shows all sentences in the conversation
    public async Task ShowAll(Conversation conversation);
}
```

### Result Display

The `LevelResultPanel` displays practice results:

```csharp
public class LevelResultPanel : MonoBehaviour
{
    // Initializes the result panel with flow result data
    public async Task Initial(FlowResult levelResult);
    
    // Displays individual practice results
    async Task ShowResults(List<PracticeResult> results);
    
    // Animates the star rating display
    async Task Rating(FlowResult flowResult);
    
    // Creates result cards for each practice
    async Task<PracticeResultPreview> CreateResultCards(PracticeResult practiceResult);
}
```

---

This documentation provides an overview of the LearningPal system architecture and components. For more detailed information on specific components or implementation details, please refer to the source code or contact the development team.
