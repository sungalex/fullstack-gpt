# 노마드코더 풀스택 GPT

**Langchain과 LLM을 이용한 앱 만들기**

- GPT 기반 앱 7개 만들기
- 자체 데이터에 GPT 활용하기
- 나만의 ChatGPT 플러그인 만들기

## 1. Introduction

- 강의 링크 : https://nomadcoders.co/fullstack-gpt

- OpenAI ChatGPT Plus 가입하기 : https://chat.openai.com/#pricing

## 2. 프로젝트 환경 구성하기

- 프로젝트 폴더 만들기 : 폴더명 `fullstack-gpt`
- git 초기화
  ```zsh
  git init .
  ```
- 가상환경 만들고 실행하기
  ```zsh
  python -m venv ./venv
  # MAC
  source venv/bin/activate
  # Windows
  ./venv/scripts/activate
  ./venv/scripts/activate.bat   # for 명령 프롬프트
  ./venv/scripts/activate.ps1   # for PowerShell
  ```
- requirements.txt 가져오기(복사) : https://gist.github.com/serranoarevalo/72d77c36dde1cc3ffec34105eb666140
- 패키지 설치하기 (python 10 버전 호환)
  ```zsh
  pip install -r requirements.txt
  ```
- `.env` 파일 만들기 : 환경변수 저장하기
  ```python
  OPENAI_API_KEY="asdf..."
  ```
- `.gitignore` 파일 만들기
  ```python
  env/
  venv/
  .env
  ```
- jupyter notebook 사용하기
  - 주피터 노트북 파일(`.ipynb` 확장자) 만들기
  - 커널 선택 : `Ctrl` + `Shift` + `P` \> `Python: select interpreter`를 찾아서 `./venv/bin/python` 선택

## 3. WELCOME TO LANGCHAIN

- LLMs and Chat Models

  ```python
  from langchain.chat_models import ChatOpenAI

  chat = ChatOpenAI(temperature=0.1)
  chat.predict("행성에 대해서 설명해줘")
  ```

- Predict Messages

  ```python
  from langchain.schema import HumanMessage, AIMessage, SystemMessage

  messages = [SystemMessage(content="당신은 전문학자 입니다. 답변은 한국어로 합니다.",),
            AIMessage(content="나는 한국의 저명한 천문학 교수 우종학 입니다.",),
            HumanMessage(content="은하와 중심 블랙홀의 연결고리에 대해 설명해줘",),
            ]

  chat.predict_messages(messages)
  ```

- Prompt Templates

  ```python
  from langchain.prompts import PromptTemplate, ChatPromptTemplate

  # String
  template = PromptTemplate.from_template("{placeholder_a}의 {placeholder_b}에 대해 설명해줘",)
  prompt = template.format(placeholder_a="태양계", placeholder_b="행성")
  chat.predict(prompt)

  # Messages
  template = ChatPromptTemplate.from_messages([
    ("system", "당신은 전문학자 입니다. 답변은 한국어로 합니다."),
    ("ai", "나는 한국의 저명한 천문학 교수 우종학 입니다."),
    ("human", "{placeholder_a}에 대해 설명해줘"),
    ])
  prompt = template.format_messages(
    language="Korean",
    placeholder_a="은하와 중심 블랙홀의 연결고리",
  )

  chat.predict_messages(prompt)
  ```

- OutputParser

  ```python
  from langchain.schema import BaseOutputParser

  class CommonOutputParser(BaseOutputParser):

    def parse(self, text):
        items = text.strip().split(",")
        return list(map(str.strip, items))
  ```

- LCEL (LangChain Expression Language)

  ```python
  template = ChatPromptTemplate.from_messages([
    ("system", "You are a list generating machine. Everything you are asked will be answered with a comma separated list of max {max_items} in lowercase. Do NOT reply with anything else."),
    ("human", "{question}"),
  ])

  chain = template | chat | CommonOutputParser()
  chain.invoke({
      "max_items": 10,
      "question": "태양계의 행성들?"
  })
  ```

- Streaming & Callbacks

  ```python
  from langchain.callbacks import StreamingStdOutCallbackHandler

  chat = ChatOpenAI(
    temperature=0.1,
    streaming=True,
    callbacks=[StreamingStdOutCallbackHandler(),]
    )
  ```

- Chaining Chains

  ```python
  chef_prompt = ChatPromptTemplate.from_messages([
      ("system", "답변은 한국어로 합니다."),
      ("human", "I want to cook {cuisine} food.")
  ])

  chef_chain = chef_prompt | chat

  veg_chef_prompt = ChatPromptTemplate.from_messages([
    ("system", "You are a vegetarian chef specialized on making traditional recipies vegetarian. You find alternative ingredients and explain their preparation. You don't radically modify the recipe. If there is no alternative for a food just say you don't know how to replace it. 답변은 한국어로 합니다."),
    ("human", "{recipe}")
  ])

  veg_chef_chain = veg_chef_prompt | chat

  final_chain = {"recipe": chef_chain} | veg_chef_chain
  final_chain.invoke({
      "cuisine": "indian"
  })
  ```

## 4. Model IO

[자세히 보기](docs/04_model_io.md)

## 5. MEMORY

[자세히 보기](docs/05_memory.md)

## 6. RAG

[자세히 보기](docs/06_rag.md)

## 7. DOCUMENTGPT

[자세히 보기](docs/07_document_gpt.md)

## 8. PRIVATEGPT

[자세히 보기](docs/08_private_gpt.md)

## 9. QUIZGPT

[자세히 보기](docs/09_quiz_gpt.md)

## 10. SITEGPT

[자세히 보기](docs/10_site_gpt.md)

## 11. MEETINGGPT

[자세히 보기](docs/11_meeting_gpt.md)

## 12. INVESTORGPT

[자세히 보기](docs/12_investor_gpt.md)

## 13. CHEFGPT

[자세히 보기](docs/13_chef_gpt.md)

## 14. ASSISTANTS API

[자세히 보기](docs/14_assistants_api.md)

## 15. AZUREGPT & AWS BEDROCK

[자세히 보기](docs/15_azure_gpt&aws_bedrock.md)
