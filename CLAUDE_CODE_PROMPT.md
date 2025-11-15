# Unity TarotMind 프로젝트 개발 프롬프트 (Claude Code용)

## 📋 프로젝트 개요

**TarotMind**는 Unity 2022.3 LTS로 개발되는 AI 기반 크로스플랫폼 타로 리딩 앱입니다.

### 기본 정보
- **플랫폼**: iOS, Android, WebGL, Windows, macOS
- **타겟**: 18-35세 한국 MZ세대
- **개발 기간**: 16개월 (MVP 12주)
- **기술 스택**: Unity 2022.3 LTS + C# 10.0 + NestJS Backend + GPT-4

---

## 🎯 클로드 코드 개발 목표

당신은 Unity 기반 TarotMind 앱을 개발하는 AI 개발자입니다. 다음 순서로 프로젝트를 구축하세요:

### Phase 1: Unity 프로젝트 초기화 (Sprint 1-2)

#### 1.1 프로젝트 구조 생성
```
TarotMind-Unity/
├── Assets/
│   ├── _Project/
│   │   ├── Scripts/
│   │   │   ├── Core/               # 핵심 시스템
│   │   │   │   ├── GameManager.cs
│   │   │   │   ├── SceneLoader.cs
│   │   │   │   └── NetworkManager.cs
│   │   │   ├── Tarot/              # 타로 로직
│   │   │   │   ├── TarotCardData.cs (ScriptableObject)
│   │   │   │   ├── TarotDeck.cs
│   │   │   │   ├── SpreadManager.cs
│   │   │   │   ├── CardInteraction.cs
│   │   │   │   └── CardShuffleManager.cs
│   │   │   ├── UI/                 # UI 스크립트
│   │   │   │   ├── Screens/
│   │   │   │   │   ├── HomeScreen.cs
│   │   │   │   │   ├── ReadingScreen.cs
│   │   │   │   │   ├── HistoryScreen.cs
│   │   │   │   │   └── SettingsScreen.cs
│   │   │   │   ├── Components/
│   │   │   │   │   ├── CardUI.cs
│   │   │   │   │   ├── SpreadLayoutUI.cs
│   │   │   │   │   └── InterpretationUI.cs
│   │   │   │   └── Animations/
│   │   │   │       ├── CardFlipAnimation.cs
│   │   │   │       └── UITransitions.cs
│   │   │   ├── AI/                 # AI 연동
│   │   │   │   ├── GPTClient.cs
│   │   │   │   └── InterpretationManager.cs
│   │   │   ├── Data/               # 데이터 모델
│   │   │   │   ├── UserData.cs
│   │   │   │   ├── ReadingData.cs
│   │   │   │   └── CardData.cs
│   │   │   └── Utils/              # 유틸리티
│   │   │       ├── APIClient.cs
│   │   │       ├── ObjectPool.cs
│   │   │       └── AudioManager.cs
│   │   ├── Scenes/
│   │   │   ├── Splash.unity
│   │   │   ├── MainMenu.unity
│   │   │   ├── Reading.unity
│   │   │   ├── History.unity
│   │   │   └── ARReading.unity
│   │   ├── Prefabs/
│   │   │   ├── Cards/              # 78장 타로 카드 프리팹
│   │   │   ├── UI/                 # UI 프리팹
│   │   │   └── Effects/            # 파티클/이펙트
│   │   ├── Materials/
│   │   ├── Shaders/
│   │   ├── Textures/
│   │   │   └── Cards/              # 78장 카드 이미지
│   │   ├── Audio/
│   │   │   ├── Voice/              # 음성 대사
│   │   │   ├── Music/
│   │   │   └── SFX/
│   │   ├── Fonts/
│   │   │   ├── Playfair-Regular.ttf
│   │   │   ├── Inter-Regular.ttf
│   │   │   └── Cinzel-Regular.ttf
│   │   └── Resources/
│   │       └── config.json
│   ├── Plugins/
│   │   ├── iOS/
│   │   └── Android/
│   ├── StreamingAssets/
│   └── AddressableAssets/
├── Packages/
│   └── manifest.json
├── ProjectSettings/
└── README.md
```

#### 1.2 Unity 설정
- **Unity Version**: 2022.3 LTS
- **Scripting Backend**: IL2CPP (성능 최적화)
- **API Compatibility Level**: .NET Standard 2.1
- **Color Space**: Linear (고품질 그래픽)
- **Rendering**: URP (Universal Render Pipeline)

#### 1.3 필수 패키지 설치
```json
{
  "dependencies": {
    "com.unity.ui-toolkit": "1.0.0",
    "com.unity.addressables": "1.21.0",
    "com.unity.xr.arfoundation": "5.0.0",
    "com.unity.purchasing": "4.5.0",
    "com.unity.mobile.notifications": "2.0.0"
  }
}
```

#### 1.4 Asset Store 플러그인
- **DOTween Pro**: 애니메이션 시스템
- **Firebase SDK**: 인증, 분석, 푸시 알림

---

### Phase 2: 타로 카드 시스템 구현 (Sprint 3-4)

#### 2.1 TarotCardData ScriptableObject 생성

```csharp
// Assets/_Project/Scripts/Tarot/TarotCardData.cs
using UnityEngine;
using System.Collections.Generic;

[CreateAssetMenu(fileName = "Card", menuName = "Tarot/Card Data")]
public class TarotCardData : ScriptableObject
{
    [Header("Card Identity")]
    public int id;                      // 0-77
    public string cardName;             // "The Fool", "Ace of Cups"
    public Arcana arcana;               // Major or Minor
    public Suit suit;                   // Cups, Wands, Swords, Pentacles, None

    [Header("Meanings")]
    [TextArea(5, 10)]
    public string uprightMeaning;

    [TextArea(5, 10)]
    public string reversedMeaning;

    public List<string> keywords;       // ["new beginnings", "innocence"]

    [Header("Visuals")]
    public Sprite frontSprite;
    public Sprite backSprite;
    public Material cardMaterial;

    [Header("Symbolism")]
    public Element element;             // Fire, Water, Air, Earth, Spirit
    public string astrologicalSign;
    public int numerology;              // 0-10
}

public enum Arcana { Major, Minor }
public enum Suit { Cups, Wands, Swords, Pentacles, None }
public enum Element { Fire, Water, Air, Earth, Spirit }
```

**작업**: 78장 타로 카드 ScriptableObject 생성
- Major Arcana: 22장 (0-21)
- Minor Arcana: 56장 (Cups 14, Wands 14, Swords 14, Pentacles 14)

#### 2.2 카드 덱 관리자

```csharp
// Assets/_Project/Scripts/Tarot/TarotDeck.cs
using System.Collections.Generic;
using System.Linq;
using UnityEngine;

public class TarotDeck : MonoBehaviour
{
    [SerializeField] private List<TarotCardData> allCards;
    private List<TarotCardData> shuffledDeck;
    private System.Random random = new System.Random();

    void Start()
    {
        LoadAllCards();
        ShuffleDeck();
    }

    private void LoadAllCards()
    {
        // Addressables에서 모든 카드 로드
        allCards = Resources.LoadAll<TarotCardData>("Cards").ToList();

        if (allCards.Count != 78)
        {
            Debug.LogError($"Expected 78 cards, found {allCards.Count}");
        }
    }

    public void ShuffleDeck()
    {
        shuffledDeck = allCards.OrderBy(x => random.Next()).ToList();
        Debug.Log("Deck shuffled");
    }

    public List<TarotCardData> DrawCards(int count)
    {
        if (count > shuffledDeck.Count)
        {
            ShuffleDeck();
        }

        var drawnCards = shuffledDeck.Take(count).ToList();
        shuffledDeck.RemoveRange(0, count);

        return drawnCards;
    }

    public TarotCardData GetRandomCard()
    {
        return allCards[random.Next(allCards.Count)];
    }
}
```

#### 2.3 스프레드 시스템

```csharp
// Assets/_Project/Scripts/Tarot/SpreadManager.cs
using System.Collections.Generic;
using UnityEngine;

[System.Serializable]
public class CardPosition
{
    public string positionName;         // "Past", "Present", "Future"
    public Vector3 worldPosition;       // 3D 공간 위치
    public string meaning;              // 이 포지션의 의미
}

[CreateAssetMenu(fileName = "Spread", menuName = "Tarot/Spread")]
public class SpreadData : ScriptableObject
{
    public string spreadName;           // "Three Card Spread"
    public int cardCount;               // 3
    public List<CardPosition> positions;

    [TextArea(3, 5)]
    public string description;

    public bool isPremium;              // 프리미엄 스프레드 여부
}

public class SpreadManager : MonoBehaviour
{
    [SerializeField] private SpreadData currentSpread;
    [SerializeField] private Transform spreadContainer;

    public void SetupSpread(SpreadData spread)
    {
        currentSpread = spread;
        PositionCardSlots();
    }

    private void PositionCardSlots()
    {
        // 스프레드에 맞게 카드 슬롯 배치
        for (int i = 0; i < currentSpread.positions.Count; i++)
        {
            Vector3 pos = currentSpread.positions[i].worldPosition;
            // 카드 슬롯 생성 및 배치
        }
    }
}
```

---

### Phase 3: 물리 기반 카드 인터랙션 (Sprint 3-4)

#### 3.1 Physics-Based Shuffle

```csharp
// Assets/_Project/Scripts/Tarot/CardShuffleManager.cs
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using DG.Tweening;

public class CardShuffleManager : MonoBehaviour
{
    [SerializeField] private GameObject cardPrefab;
    [SerializeField] private int cardCount = 78;
    [SerializeField] private float shuffleForce = 5f;
    [SerializeField] private float arrangeDelay = 3f;

    private List<GameObject> cards = new List<GameObject>();

    public void ShuffleCards()
    {
        // 기존 카드 제거
        ClearCards();

        // 카드 생성 및 물리 기반 섞기
        for (int i = 0; i < cardCount; i++)
        {
            Vector3 randomPos = new Vector3(
                Random.Range(-2f, 2f),
                Random.Range(1f, 3f),
                Random.Range(-2f, 2f)
            );

            Quaternion randomRot = Random.rotation;

            GameObject card = Instantiate(cardPrefab, randomPos, randomRot);
            Rigidbody rb = card.GetComponent<Rigidbody>();

            // 랜덤 힘 적용 (섞는 효과)
            rb.AddForce(Random.insideUnitSphere * shuffleForce, ForceMode.Impulse);
            rb.AddTorque(Random.insideUnitSphere * 3f, ForceMode.Impulse);

            cards.Add(card);
        }

        // 3초 후 카드 정렬
        StartCoroutine(ArrangeCardsAfterShuffle(arrangeDelay));
    }

    IEnumerator ArrangeCardsAfterShuffle(float delay)
    {
        yield return new WaitForSeconds(delay);

        // Rigidbody 비활성화 (애니메이션 제어를 위해)
        foreach (var card in cards)
        {
            Rigidbody rb = card.GetComponent<Rigidbody>();
            rb.isKinematic = true;
        }

        // DOTween으로 카드들을 정렬된 위치로 이동
        for (int i = 0; i < cards.Count; i++)
        {
            Vector3 targetPos = GetSpreadPosition(i);
            Quaternion targetRot = Quaternion.Euler(0, 180, 0);

            cards[i].transform.DOMove(targetPos, 1f)
                .SetEase(Ease.OutQuad);

            cards[i].transform.DORotateQuaternion(targetRot, 1f)
                .SetEase(Ease.OutQuad);
        }

        yield return new WaitForSeconds(1f);

        // 카드 선택 가능 상태로
        EnableCardSelection();
    }

    private Vector3 GetSpreadPosition(int index)
    {
        // 부채꼴 형태로 배치
        float angle = (index - cardCount / 2f) * 2f;
        float radius = 5f;

        return new Vector3(
            Mathf.Sin(angle * Mathf.Deg2Rad) * radius,
            0f,
            Mathf.Cos(angle * Mathf.Deg2Rad) * radius - 3f
        );
    }

    private void ClearCards()
    {
        foreach (var card in cards)
        {
            Destroy(card);
        }
        cards.Clear();
    }

    private void EnableCardSelection()
    {
        foreach (var card in cards)
        {
            CardInteraction interaction = card.GetComponent<CardInteraction>();
            interaction.EnableSelection();
        }
    }
}
```

#### 3.2 3D Card Flip Animation

```csharp
// Assets/_Project/Scripts/Tarot/CardInteraction.cs
using UnityEngine;
using DG.Tweening;
using UnityEngine.Events;

public class CardInteraction : MonoBehaviour
{
    [SerializeField] private TarotCardData cardData;
    [SerializeField] private MeshRenderer cardRenderer;
    [SerializeField] private Material frontMaterial;
    [SerializeField] private Material backMaterial;

    private bool isRevealed = false;
    private bool isSelectable = false;

    public UnityEvent<TarotCardData> OnCardRevealed;

    void OnMouseDown()
    {
        if (isSelectable && !isRevealed)
        {
            RevealCard();
        }
    }

    public void RevealCard()
    {
        isRevealed = true;

        // 3D Flip 애니메이션 (Y축 180도 회전)
        transform.DORotate(new Vector3(0, 180, 0), 0.6f)
            .SetEase(Ease.OutBack)
            .OnUpdate(() => {
                // 90도 지점에서 앞면/뒷면 전환
                if (transform.eulerAngles.y > 90 && cardRenderer.material == backMaterial)
                {
                    cardRenderer.material = frontMaterial;
                    cardRenderer.material.mainTexture = cardData.frontSprite.texture;
                }
            })
            .OnComplete(() => {
                // 카드 공개 완료 이벤트
                OnCardRevealed?.Invoke(cardData);

                // 파티클 효과
                PlayRevealEffect();
            });

        // 위로 살짝 띄우는 애니메이션
        transform.DOMoveY(transform.position.y + 0.5f, 0.3f)
            .SetEase(Ease.OutQuad);
    }

    private void PlayRevealEffect()
    {
        // 파티클 시스템 재생
        ParticleSystem ps = GetComponentInChildren<ParticleSystem>();
        if (ps != null)
        {
            ps.Play();
        }
    }

    public void EnableSelection()
    {
        isSelectable = true;

        // Hover 효과 추가
        AddHoverEffect();
    }

    private void AddHoverEffect()
    {
        // 계속 떠있는 애니메이션
        transform.DOLocalMoveY(transform.localPosition.y + 0.1f, 1f)
            .SetEase(Ease.InOutSine)
            .SetLoops(-1, LoopType.Yoyo);
    }
}
```

---

### Phase 4: AI 연동 (Sprint 5-6)

#### 4.1 API Client

```csharp
// Assets/_Project/Scripts/Utils/APIClient.cs
using System;
using System.Collections;
using System.Text;
using UnityEngine;
using UnityEngine.Networking;

[System.Serializable]
public class ReadingRequest
{
    public int[] cardIds;
    public string question;
    public string spreadType;
}

[System.Serializable]
public class InterpretationResponse
{
    public string interpretation;
    public float sentiment;
    public string[] keywords;
}

public class APIClient : MonoBehaviour
{
    private const string BASE_URL = "http://localhost:3000/api";
    private string authToken;

    public void SetAuthToken(string token)
    {
        authToken = token;
    }

    public IEnumerator GetInterpretation(
        int[] cardIds,
        string question,
        string spreadType,
        Action<InterpretationResponse> onSuccess,
        Action<string> onError)
    {
        string url = $"{BASE_URL}/readings";

        // Request 객체 생성
        ReadingRequest request = new ReadingRequest
        {
            cardIds = cardIds,
            question = question,
            spreadType = spreadType
        };

        string jsonData = JsonUtility.ToJson(request);
        byte[] bodyRaw = Encoding.UTF8.GetBytes(jsonData);

        using (UnityWebRequest www = new UnityWebRequest(url, "POST"))
        {
            www.uploadHandler = new UploadHandlerRaw(bodyRaw);
            www.downloadHandler = new DownloadHandlerBuffer();

            // Headers
            www.SetRequestHeader("Content-Type", "application/json");
            www.SetRequestHeader("Authorization", $"Bearer {authToken}");

            yield return www.SendWebRequest();

            if (www.result == UnityWebRequest.Result.Success)
            {
                InterpretationResponse response =
                    JsonUtility.FromJson<InterpretationResponse>(www.downloadHandler.text);
                onSuccess?.Invoke(response);
            }
            else
            {
                Debug.LogError($"API Error: {www.error}");
                onError?.Invoke(www.error);
            }
        }
    }

    public IEnumerator Login(string email, string password,
        Action<string> onSuccess, Action<string> onError)
    {
        string url = $"{BASE_URL}/auth/login";

        string jsonData = $"{{\"email\":\"{email}\",\"password\":\"{password}\"}}";
        byte[] bodyRaw = Encoding.UTF8.GetBytes(jsonData);

        using (UnityWebRequest www = new UnityWebRequest(url, "POST"))
        {
            www.uploadHandler = new UploadHandlerRaw(bodyRaw);
            www.downloadHandler = new DownloadHandlerBuffer();
            www.SetRequestHeader("Content-Type", "application/json");

            yield return www.SendWebRequest();

            if (www.result == UnityWebRequest.Result.Success)
            {
                // 토큰 추출
                var response = JsonUtility.FromJson<LoginResponse>(www.downloadHandler.text);
                authToken = response.token;
                onSuccess?.Invoke(authToken);
            }
            else
            {
                onError?.Invoke(www.error);
            }
        }
    }
}

[System.Serializable]
public class LoginResponse
{
    public string token;
    public UserData user;
}
```

#### 4.2 Interpretation Manager

```csharp
// Assets/_Project/Scripts/AI/InterpretationManager.cs
using System.Collections.Generic;
using UnityEngine;
using TMPro;

public class InterpretationManager : MonoBehaviour
{
    [SerializeField] private APIClient apiClient;
    [SerializeField] private TextMeshProUGUI interpretationText;
    [SerializeField] private GameObject loadingIndicator;

    public void RequestInterpretation(List<TarotCardData> cards, string question)
    {
        // 카드 ID 배열 생성
        int[] cardIds = new int[cards.Count];
        for (int i = 0; i < cards.Count; i++)
        {
            cardIds[i] = cards[i].id;
        }

        // 로딩 표시
        loadingIndicator.SetActive(true);
        interpretationText.text = "AI가 카드를 해석하고 있습니다...";

        // API 호출
        StartCoroutine(apiClient.GetInterpretation(
            cardIds,
            question,
            "three-card",
            OnInterpretationReceived,
            OnError
        ));
    }

    private void OnInterpretationReceived(InterpretationResponse response)
    {
        loadingIndicator.SetActive(false);

        // 타이핑 효과로 텍스트 표시
        StartCoroutine(TypeText(response.interpretation, 0.03f));
    }

    private void OnError(string error)
    {
        loadingIndicator.SetActive(false);
        interpretationText.text = $"오류가 발생했습니다: {error}";
    }

    private IEnumerator TypeText(string text, float delay)
    {
        interpretationText.text = "";

        foreach (char c in text)
        {
            interpretationText.text += c;
            yield return new WaitForSeconds(delay);
        }
    }
}
```

---

### Phase 5: UI Toolkit 화면 구현 (Sprint 7-8)

#### 5.1 USS 스타일시트 (Celestial Harmony Theme)

```css
/* Assets/_Project/UI/Styles/main.uss */

/* === Colors === */
:root {
    --primary-purple: #6B46C1;
    --primary-gold: #F59E0B;
    --primary-midnight: #1E293B;
    --mystic-blue: #3B82F6;
    --cosmic-pink: #EC4899;
    --gray-900: #111827;
    --gray-100: #F3F4F6;
}

/* === Card Container === */
.card-container {
    width: 200px;
    height: 350px;
    border-radius: 10px;
    background-color: rgba(107, 70, 193, 0.1);
    transition-duration: 0.3s;
    margin: 10px;
}

.card-container:hover {
    scale: 1.05;
    background-color: rgba(107, 70, 193, 0.2);
}

/* === Reading Screen === */
.reading-screen {
    flex-direction: column;
    align-items: center;
    justify-content: center;
    padding: 20px;
    background-image: url('project://Assets/_Project/Textures/UI/background_stars.png');
}

.screen-title {
    font-size: 32px;
    color: #F59E0B;
    -unity-font: url('project://Assets/_Project/Fonts/Playfair-Regular.ttf');
    margin-bottom: 20px;
}

/* === Interpretation Text === */
.interpretation-text {
    font-size: 16px;
    color: #F3F4F6;
    -unity-font: url('project://Assets/_Project/Fonts/Cinzel-Regular.ttf');
    white-space: normal;
    padding: 20px;
    background-color: rgba(30, 41, 59, 0.8);
    border-radius: 10px;
    max-width: 600px;
}

/* === Button Styles === */
.primary-button {
    background-color: #6B46C1;
    color: #F3F4F6;
    border-radius: 8px;
    padding: 12px 24px;
    font-size: 16px;
    -unity-font: url('project://Assets/_Project/Fonts/Inter-Regular.ttf');
    transition-duration: 0.2s;
}

.primary-button:hover {
    background-color: #553C9A;
    scale: 1.05;
}

.primary-button:active {
    scale: 0.95;
}

/* === Card Spread Layout === */
.card-spread {
    flex-direction: row;
    justify-content: space-around;
    align-items: center;
    width: 100%;
    margin: 30px 0;
}
```

#### 5.2 UXML 레이아웃 (Reading Screen)

```xml
<!-- Assets/_Project/UI/Screens/ReadingScreen.uxml -->
<ui:UXML xmlns:ui="UnityEngine.UIElements">
    <ui:VisualElement class="reading-screen">
        <!-- Title -->
        <ui:Label text="타로 리딩" class="screen-title"/>

        <!-- Question Input -->
        <ui:VisualElement style="margin: 20px 0; width: 80%;">
            <ui:Label text="질문을 입력하세요" style="font-size: 18px; margin-bottom: 10px;"/>
            <ui:TextField name="question-input"
                         placeholder-text="마음속 질문을 떠올려보세요..."
                         multiline="true"
                         style="min-height: 80px; font-size: 16px;"/>
        </ui:VisualElement>

        <!-- Shuffle Button -->
        <ui:Button name="shuffle-button" text="카드 섞기" class="primary-button"
                  style="margin: 20px 0;"/>

        <!-- Card Spread Container -->
        <ui:VisualElement name="card-spread" class="card-spread">
            <!-- 카드 슬롯은 런타임에 동적 생성 -->
        </ui:VisualElement>

        <!-- Interpretation Section -->
        <ui:ScrollView name="interpretation-container"
                      style="width: 80%; min-height: 300px; margin-top: 30px;">
            <ui:Label name="interpretation-text"
                     class="interpretation-text"
                     text="카드를 선택하면 해석이 표시됩니다."/>
        </ui:ScrollView>

        <!-- Action Buttons -->
        <ui:VisualElement style="flex-direction: row; margin-top: 20px;">
            <ui:Button name="save-button" text="저장" class="primary-button"
                      style="margin-right: 10px;"/>
            <ui:Button name="share-button" text="공유" class="primary-button"/>
        </ui:VisualElement>
    </ui:VisualElement>
</ui:UXML>
```

#### 5.3 C# UI Controller

```csharp
// Assets/_Project/Scripts/UI/Screens/ReadingScreen.cs
using UnityEngine;
using UnityEngine.UIElements;
using System.Collections.Generic;

public class ReadingScreen : MonoBehaviour
{
    [SerializeField] private UIDocument uiDocument;
    [SerializeField] private CardShuffleManager shuffleManager;
    [SerializeField] private InterpretationManager interpretationManager;

    private VisualElement root;
    private Button shuffleButton;
    private TextField questionInput;
    private VisualElement cardSpread;
    private Label interpretationText;

    void OnEnable()
    {
        root = uiDocument.rootVisualElement;

        // UI 요소 바인딩
        shuffleButton = root.Q<Button>("shuffle-button");
        questionInput = root.Q<TextField>("question-input");
        cardSpread = root.Q<VisualElement>("card-spread");
        interpretationText = root.Q<Label>("interpretation-text");

        // 이벤트 등록
        shuffleButton.clicked += OnShuffleClicked;

        root.Q<Button>("save-button").clicked += OnSaveClicked;
        root.Q<Button>("share-button").clicked += OnShareClicked;
    }

    void OnDisable()
    {
        shuffleButton.clicked -= OnShuffleClicked;
    }

    private void OnShuffleClicked()
    {
        string question = questionInput.value;

        if (string.IsNullOrEmpty(question))
        {
            interpretationText.text = "질문을 입력해주세요.";
            return;
        }

        // 3D 공간에서 카드 섞기 실행
        shuffleManager.ShuffleCards();

        // UI 업데이트
        interpretationText.text = "카드를 섞는 중...";
        shuffleButton.SetEnabled(false);
    }

    public void OnCardsDrawn(List<TarotCardData> cards)
    {
        // 카드가 뽑혔을 때 호출됨
        string question = questionInput.value;
        interpretationManager.RequestInterpretation(cards, question);
    }

    private void OnSaveClicked()
    {
        // 로컬 저장 + 클라우드 동기화
        SaveReadingToCloud();
    }

    private void OnShareClicked()
    {
        // 소셜 공유
        ShareReading();
    }

    private void SaveReadingToCloud()
    {
        // PlayerPrefs 또는 클라우드 저장
        Debug.Log("Reading saved");
    }

    private void ShareReading()
    {
        // Native Share Plugin 사용
        Debug.Log("Share reading");
    }
}
```

---

### Phase 6: AR Foundation 통합 (Sprint 9-10)

#### 6.1 AR Card Reading

```csharp
// Assets/_Project/Scripts/AR/ARCardManager.cs
using UnityEngine;
using UnityEngine.XR.ARFoundation;
using UnityEngine.XR.ARSubsystems;
using System.Collections.Generic;

[RequireComponent(typeof(ARRaycastManager))]
[RequireComponent(typeof(ARPlaneManager))]
public class ARCardManager : MonoBehaviour
{
    [SerializeField] private GameObject cardPrefab;
    [SerializeField] private ARRaycastManager raycastManager;
    [SerializeField] private ARPlaneManager planeManager;

    private List<ARRaycastHit> hits = new List<ARRaycastHit>();
    private List<GameObject> spawnedCards = new List<GameObject>();

    void Update()
    {
        // 터치 입력 감지
        if (Input.touchCount > 0)
        {
            Touch touch = Input.GetTouch(0);

            if (touch.phase == TouchPhase.Began)
            {
                // AR Raycast로 평면 감지
                if (raycastManager.Raycast(touch.position, hits, TrackableType.PlaneWithinPolygon))
                {
                    Pose hitPose = hits[0].pose;

                    // 카드 배치
                    SpawnCardAt(hitPose.position, hitPose.rotation);
                }
            }
        }
    }

    private void SpawnCardAt(Vector3 position, Quaternion rotation)
    {
        if (spawnedCards.Count >= 3)
        {
            Debug.Log("Already drew 3 cards");
            return;
        }

        GameObject card = Instantiate(cardPrefab, position, rotation);
        spawnedCards.Add(card);

        Debug.Log($"Spawned card at {position}");
    }

    public void ClearCards()
    {
        foreach (var card in spawnedCards)
        {
            Destroy(card);
        }
        spawnedCards.Clear();
    }
}
```

---

### Phase 7: 인앱 결제 (Sprint 9-10)

#### 7.1 Unity IAP 설정

```csharp
// Assets/_Project/Scripts/Core/IAPManager.cs
using UnityEngine;
using UnityEngine.Purchasing;
using System;

public class IAPManager : MonoBehaviour, IStoreListener
{
    private static IStoreController storeController;
    private static IExtensionProvider storeExtensionProvider;

    // Product IDs
    private const string PRODUCT_PLUS_MONTHLY = "com.tarotmind.plus.monthly";
    private const string PRODUCT_PREMIUM_MONTHLY = "com.tarotmind.premium.monthly";
    private const string PRODUCT_COINS_1100 = "com.tarotmind.coins.1100";
    private const string PRODUCT_COINS_5500 = "com.tarotmind.coins.5500";

    void Start()
    {
        InitializePurchasing();
    }

    public void InitializePurchasing()
    {
        if (IsInitialized()) return;

        var builder = ConfigurationBuilder.Instance(StandardPurchasingModule.Instance());

        // 구독 상품
        builder.AddProduct(PRODUCT_PLUS_MONTHLY, ProductType.Subscription);
        builder.AddProduct(PRODUCT_PREMIUM_MONTHLY, ProductType.Subscription);

        // 코인 상품 (소모품)
        builder.AddProduct(PRODUCT_COINS_1100, ProductType.Consumable);
        builder.AddProduct(PRODUCT_COINS_5500, ProductType.Consumable);

        UnityPurchasing.Initialize(this, builder);
    }

    private bool IsInitialized()
    {
        return storeController != null && storeExtensionProvider != null;
    }

    public void BuyPlusSubscription()
    {
        BuyProductID(PRODUCT_PLUS_MONTHLY);
    }

    public void BuyPremiumSubscription()
    {
        BuyProductID(PRODUCT_PREMIUM_MONTHLY);
    }

    public void BuyCoins(int amount)
    {
        string productId = amount == 1100 ? PRODUCT_COINS_1100 : PRODUCT_COINS_5500;
        BuyProductID(productId);
    }

    void BuyProductID(string productId)
    {
        if (IsInitialized())
        {
            Product product = storeController.products.WithID(productId);

            if (product != null && product.availableToPurchase)
            {
                Debug.Log($"Purchasing product: {product.definition.id}");
                storeController.InitiatePurchase(product);
            }
            else
            {
                Debug.Log("Product not available for purchase");
            }
        }
        else
        {
            Debug.Log("IAP not initialized");
        }
    }

    // IStoreListener 구현
    public void OnInitialized(IStoreController controller, IExtensionProvider extensions)
    {
        Debug.Log("IAP Initialized");
        storeController = controller;
        storeExtensionProvider = extensions;
    }

    public void OnInitializeFailed(InitializationFailureReason error)
    {
        Debug.Log($"IAP Initialization Failed: {error}");
    }

    public PurchaseProcessingResult ProcessPurchase(PurchaseEventArgs args)
    {
        Debug.Log($"Purchase successful: {args.purchasedProduct.definition.id}");

        // 백엔드에 구매 검증 요청
        VerifyPurchaseWithBackend(args.purchasedProduct);

        return PurchaseProcessingResult.Complete;
    }

    public void OnPurchaseFailed(Product product, PurchaseFailureReason failureReason)
    {
        Debug.Log($"Purchase failed: {product.definition.id}, Reason: {failureReason}");
    }

    private void VerifyPurchaseWithBackend(Product product)
    {
        // 백엔드 API 호출하여 영수증 검증
        // iOS: App Store Receipt
        // Android: Google Play Receipt
    }
}
```

---

### Phase 8: 성능 최적화 (Sprint 11-12)

#### 8.1 Object Pooling

```csharp
// Assets/_Project/Scripts/Utils/ObjectPool.cs
using System.Collections.Generic;
using UnityEngine;

public class ObjectPool : MonoBehaviour
{
    [System.Serializable]
    public class Pool
    {
        public string tag;
        public GameObject prefab;
        public int size;
    }

    public List<Pool> pools;
    private Dictionary<string, Queue<GameObject>> poolDictionary;

    void Start()
    {
        poolDictionary = new Dictionary<string, Queue<GameObject>>();

        foreach (Pool pool in pools)
        {
            Queue<GameObject> objectPool = new Queue<GameObject>();

            for (int i = 0; i < pool.size; i++)
            {
                GameObject obj = Instantiate(pool.prefab);
                obj.SetActive(false);
                objectPool.Enqueue(obj);
            }

            poolDictionary.Add(pool.tag, objectPool);
        }
    }

    public GameObject SpawnFromPool(string tag, Vector3 position, Quaternion rotation)
    {
        if (!poolDictionary.ContainsKey(tag))
        {
            Debug.LogWarning($"Pool with tag {tag} doesn't exist");
            return null;
        }

        GameObject objectToSpawn = poolDictionary[tag].Dequeue();

        objectToSpawn.SetActive(true);
        objectToSpawn.transform.position = position;
        objectToSpawn.transform.rotation = rotation;

        poolDictionary[tag].Enqueue(objectToSpawn);

        return objectToSpawn;
    }
}
```

#### 8.2 Addressables 리소스 관리

```csharp
// Assets/_Project/Scripts/Utils/CardResourceManager.cs
using UnityEngine;
using UnityEngine.AddressableAssets;
using UnityEngine.ResourceManagement.AsyncOperations;
using System.Threading.Tasks;

public class CardResourceManager : MonoBehaviour
{
    public async Task<TarotCardData> LoadCardAsync(int cardId)
    {
        string address = $"Cards/Card_{cardId:D2}";

        AsyncOperationHandle<TarotCardData> handle =
            Addressables.LoadAssetAsync<TarotCardData>(address);

        await handle.Task;

        if (handle.Status == AsyncOperationStatus.Succeeded)
        {
            return handle.Result;
        }

        Debug.LogError($"Failed to load card: {cardId}");
        return null;
    }

    public async Task<Sprite> LoadCardSpriteAsync(int cardId)
    {
        string address = $"CardSprites/Card_{cardId:D2}";

        AsyncOperationHandle<Sprite> handle =
            Addressables.LoadAssetAsync<Sprite>(address);

        await handle.Task;

        if (handle.Status == AsyncOperationStatus.Succeeded)
        {
            return handle.Result;
        }

        Debug.LogError($"Failed to load card sprite: {cardId}");
        return null;
    }

    void OnDestroy()
    {
        // Addressables 리소스 해제
        Addressables.ReleaseInstance(gameObject);
    }
}
```

---

## 🎯 개발 우선순위

### 🔴 Critical (즉시 개발)
1. ✅ Unity 프로젝트 초기화
2. ✅ 78장 타로 카드 ScriptableObject 생성
3. ✅ TarotDeck 및 SpreadManager 구현
4. ✅ Physics-based 카드 섞기
5. ✅ 3D Card Flip 애니메이션

### 🟠 High (MVP 필수)
6. ✅ GPT-4 API 연동 (APIClient)
7. ✅ InterpretationManager 구현
8. ✅ UI Toolkit 기반 ReadingScreen
9. ✅ 로컬 저장 (PlayerPrefs + 클라우드)

### 🟡 Medium (런칭 준비)
10. ⬜ AR Foundation 통합
11. ⬜ Unity IAP 구독/코인 시스템
12. ⬜ Object Pooling 성능 최적화
13. ⬜ Addressables 리소스 관리

### 🟢 Low (확장 기능)
14. ⬜ 음성 안내 (ElevenLabs API)
15. ⬜ 커뮤니티 기능
16. ⬜ 다국어 지원

---

## 📊 성공 지표

### 기술적 KPI
- [ ] Unity Profiler에서 60fps 유지 (모바일)
- [ ] 빌드 크기 < 150MB (Addressables 적용 후)
- [ ] 메모리 사용량 < 500MB
- [ ] API 응답 시간 < 2초

### 기능적 체크리스트
- [ ] 78장 타로 카드 구현
- [ ] 최소 3가지 스프레드 (1카드, 3카드, 켈틱 크로스)
- [ ] GPT-4 해석 생성
- [ ] AR 카드 뽑기
- [ ] 구독/코인 시스템
- [ ] 리딩 히스토리

---

## 🚀 시작 명령어

```bash
# 1. Unity 프로젝트 생성
Unity Hub → New Project → 3D (URP) → TarotMind-Unity

# 2. 필수 패키지 설치
Window → Package Manager → 다음 패키지 설치:
- UI Toolkit
- Addressables
- AR Foundation
- In-App Purchasing
- Mobile Notifications

# 3. Asset Store 플러그인
- DOTween Pro (애니메이션)
- Firebase SDK (인증, 분석)

# 4. 프로젝트 설정
Edit → Project Settings:
- Player → IL2CPP
- Graphics → URP Asset
- Quality → 60fps

# 5. 개발 시작
위 Phase 1부터 순차적으로 구현
```

---

## 📝 참고 문서

- **전체 로드맵**: `Step0_전체_로드맵/01_TarotMind_개발_로드맵_통합본.md`
- **기술 스펙**: `Step2_기술_설계/01_타로앱_개발기획서.md`
- **QuickStart**: `Step4_개발_실행/02_타로앱_QuickStart_Guide.md`
- **UX 플로우**: `Step3_UX_UI_디자인/02_타로리딩_UX플로우_화면설계서.md`

---

**개발 기간**: 12주 (MVP)
**목표**: iOS/Android 정식 런칭
**핵심 기술**: Unity 2022.3 LTS + GPT-4 + AR Foundation
