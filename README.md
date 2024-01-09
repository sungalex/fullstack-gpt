## 노마드코더 풀스택 GPT

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
