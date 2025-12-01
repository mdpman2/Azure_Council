# Azure LLM Council Framework

이 프로젝트는 단일 LLM(Large Language Model)의 한계를 극복하고, 집단 지성을 활용하여 최적의 답변을 도출하기 위한 **"Council of Experts(전문가 위원회)"** 아키텍처를 구현한 Python 프레임워크입니다.

## 🧠 왜 'LLM Council'이 필요한가? (Logical Reasoning)

단일 모델에 의존하는 것은 마치 한 명의 전문가에게만 자문을 구하는 것과 같습니다. 아무리 뛰어난 전문가라도 실수를 하거나, 편향된 시각을 가질 수 있습니다. 이 프레임워크는 다음과 같은 논리적 이점을 제공합니다:

1.  **사고의 다양성 (Diversity of Thought)**:
    -   OpenAI의 GPT 계열과 Anthropic의 Claude 계열은 서로 다른 학습 데이터와 추론 방식을 가집니다.
    -   GPT가 논리적 구조와 코딩에 강하다면, Claude는 문맥 파악과 창의적 글쓰기에 강점이 있을 수 있습니다. 이들을 결합하면 상호 보완적인 결과를 얻을 수 있습니다.

2.  **상호 검증 및 오류 수정 (Peer Review & Error Correction)**:
    -   Stage 2의 '상호 리뷰' 과정에서 각 모델은 다른 모델의 답변을 비판적으로 평가합니다.
    -   이를 통해 환각(Hallucination)이나 사실적 오류를 식별하고 걸러낼 수 있습니다.

3.  **최적의 합성 (Optimal Synthesis)**:
    -   단순히 여러 답변을 나열하는 것이 아니라, '의장(Chairman)' 모델이 각 답변의 장점만을 취합하여 최종 결론을 내립니다.
    -   이는 개별 모델의 단순 합보다 더 높은 품질의 통찰력을 제공합니다.

---

## 🤖 참여 모델 비교 (Council Members)

이 프레임워크는 현재 다음 5개의 최첨단 모델을 위원회 멤버로 구성하고 있습니다:

| 모델명 | 제공사 (Provider) | 역할 및 특징 |
| :--- | :--- | :--- |
| **GPT-5.1** | OpenAI | **👑 의장 (Chairman)**<br>가장 높은 추론 능력을 가진 모델로 설정되어, 모든 답변과 리뷰를 종합하여 최종 의사결정을 내리는 역할을 수행합니다. |
| **GPT-5** | OpenAI | **위원 (Member)**<br>최신 GPT 아키텍처를 기반으로 강력한 일반 추론 능력을 제공합니다. |
| **GPT-4.1** | OpenAI | **위원 (Member)**<br>안정적이고 검증된 성능을 바탕으로 균형 잡힌 시각을 제공합니다. |
| **Claude Opus 4.5** | Anthropic | **위원 (Member)**<br>복잡한 추론과 긴 문맥 이해에 탁월하며, GPT 계열과는 다른 관점의 통찰력을 제시합니다. |
| **Claude Sonnet 4.5** | Anthropic | **위원 (Member)**<br>빠르고 효율적인 처리가 특징이며, 실용적이고 간결한 답변을 제공하는 경향이 있습니다. |

---

## 🔄 작동 프로세스 (Process Flow)

전체 프로세스는 3단계(Stage)로 구성되며, Python의 `asyncio`를 통해 비동기적으로 빠르게 처리됩니다.

### **Stage 1: 독립적 의견 제시 (First Opinions)**
-   사용자의 질문이 5개의 모델에게 동시에 전달됩니다.
-   각 모델은 다른 모델의 존재를 모른 채, 자신만의 지식과 논리로 최선의 답변을 생성합니다.

### **Stage 2: 상호 블라인드 리뷰 (Blind Peer Review)**
-   생성된 5개의 답변이 익명화되어 다시 모든 모델에게 전달됩니다.
-   각 모델은 심사위원이 되어 답변들의 **정확성**과 **통찰력**을 평가하고, 순위(Ranking)를 매기며, 장단점을 분석한 코멘트를 작성합니다.

### **Stage 3: 의장의 최종 종합 (Chairman's Synthesis)**
-   **의장 모델(GPT-5.1)**은 다음 정보를 모두 수신합니다:
    1.  원래의 사용자 질문
    2.  5개 모델의 개별 답변
    3.  5개 모델의 상호 리뷰 결과 (랭킹 및 코멘트)
-   의장은 이 모든 정보를 종합하여, 비판을 수용하고 장점을 결합한 **최종 마스터 답변**을 작성합니다.

---

## 🚀 시작하기

### 사전 요구 사항
-   Python 3.8+
-   Azure OpenAI 및 Azure Anthropic 리소스 접근 권한
-   `.env` 파일에 API 키 및 엔드포인트 설정

### 실행 방법

```bash
# 의존성 설치
pip install aiohttp python-dotenv

# 스크립트 실행
python azure_council_full.py
```

### 환경 변수 설정 (.env)

```ini
OPEN_AI_KEY_5=your_api_key_here
OPEN_AI_ENDPOINT_5=https://your-resource-name.openai.azure.com/
# 필요한 경우 배포명 변경 가능
AZURE_DEPLOY_GPT5=gpt-5
AZURE_DEPLOY_GPT5_1=gpt-5.1
...
```
