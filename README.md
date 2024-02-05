# 노마드코더 풀스택 GPT

- https://nomadcoders.co/fullstack-gpt/lobby

## OpenAI ChatGPT Plus 가입하기

- https://chat.openai.com/#pricing

## 프로젝트 환경 구성하기

- 프로젝트 폴더 만들기 : 폴더명 `fullstack-gpt`
- git 초기화
  ```zsh
  git init .
  ```
- 가상환경 만들고 실행하기
  ```zsh
  python -m venv ./env
  source env/bin/activate
  ```
- requirements.txt 가져오기(복사) : https://gist.github.com/serranoarevalo/72d77c36dde1cc3ffec34105eb666140
- 패키지 설치하기
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
  .env
  ```
- jupyter notebook 사용하기
  - 주피터 노트북 파일(`.ipynb` 확장자) 만들기
  - 커널 선택 : `env/bin/python`

## WELCOME TO LANGCHAIN

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
