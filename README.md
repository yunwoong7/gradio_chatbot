<h2 align="center">
Gradio Chatbot
</h2>

<div align="center">
  <img src="https://img.shields.io/badge/python-v3.9.16-blue.svg"/>
  <img src="https://img.shields.io/badge/gradio-v3.24.0-blue.svg"/>
  <img src="https://img.shields.io/badge/gradio_client-v0.0.5-blue.svg"/>
</div>



OpenAI가 ChatGPT를 내놓은 이후 전 세계가 들썩이고 있습니다. 다양한 기업들이 자신만의 기술과 ChatGPT를 접목한 사업한 사업을 선보이고 있습니다. Chatbot은 고객과 최종 사용자가 직접 소통하도록 설계되었기 때문에 다양한 입력 프롬프트에 직면했을 때 Chatbot이 예상대로 작동하는지 확인하는 것이 매우 중요합니다. gradio는 Chatbot 데모를 쉽게 구축하고 GUI를 통해 직접 테스트할 수 있습니다.

<div align="center">
<img src="https://blog.kakaocdn.net/dn/bslJDp/btr7n1ZM3ZO/HKtiyX3x3oXb6h3czcEr8K/img.gif" width="70%">
</div>

 OpenAI의 GPT-3.5 모델로 챗봇 프로그램을 만들어 보겠습니다.

------

### 1. 설치 (Installation)

gradio 패키지 설치는 pip 명령어를 이용하여 설치할 수 있습니다.

```bash
pip install gradio
```

gradio 설치와 관련된 내용은 이전 글을 참고하시기 바랍니다.

 

gradio 시작하기 (설치방법)

AI 학습 모델은 강력하고 매우 흥미롭지만 그 자체로만 본다면 그다지 유용해 보이지 않습니다. 모델이 완성되면 어떠한 가치를 제공할 수 있는지 증명이 필요한데 Accuracy, Precision, Recall, IOU, PSNR

yunwoong.tistory.com

### 2. GPT-3.5 챗봇

다음으로 OpenAI API를 이용한 GPT-3 챗봇을 만들도록 하겠습니다.

먼저 OpenAI API를 사용하기 위해 API 키 발급이 필요합니다. 먼저 [OpenAI API 사이트](https://platform.openai.com/)로 이동합니다. OpenAI 계정이 필요하며 계정이 없다면 계정 생성이 필요합니다. 간단히 Google이나 Microsoft 계정을 연동 할 수 있습니다. 이미 계정이 있다면 로그인 후 진행하시면 됩니다.

로그인이 되었다면 우측 상단 Personal -> [ View API Keys ] 를 클릭합니다.

![img](https://blog.kakaocdn.net/dn/dScgEh/btr7C15EKC9/EhNUvFp5baa7iJOCauKMd0/img.png)

[ + Create new secret key ] 를 클릭하여 API Key를 생성합니다. API key generated 창이 활성화되면 Key 를 반드시 복사하여 두시기 바랍니다. 창을 닫으면 다시 확인할 수 없습니다. (만약 복사하지 못했다면 다시 Create new secret key 버튼을 눌러 생성하면 되니 걱정하지 않으셔도 됩니다.)

Python 파일 chatgpt_app.py 을 생성하고 아래와 같이 작성합니다. openai.api_key는 자신의 OpenAI API Key를 입력합니다. (예: sk-xxxxxxxxxxxxxxxxxxxxx)

```python
import gradio as gr
import openai
 
openai.api_key = 'YOUR API KEY HERE'
 
def answer(state, state_chatbot, text):
    messages = state + [{
        'role': 'user',
        'content': text
    }]
 
    res = openai.ChatCompletion.create(
        model='gpt-3.5-turbo',
        messages=messages
    )
 
    msg = res['choices'][0]['message']['content']
 
    new_state = [{
        'role': 'user',
        'content': text
    }, {
        'role': 'assistant',
        'content': msg
    }]
 
    state = state + new_state
    state_chatbot = state_chatbot + [(text, msg)]
 
    print(state)
 
    return state, state_chatbot, state_chatbot
 
 
with gr.Blocks(css='#chatbot .overflow-y-auto{height:750px}') as demo:
    state = gr.State([{
        'role': 'system',
        'content': 'You are a helpful assistant.'
    }])
    state_chatbot = gr.State([])
 
    with gr.Row():
        gr.HTML("""<div style="text-align: center; max-width: 500px; margin: 0 auto;">
            <div>
                <h1>Yunwoong's ChatGPT-3.5</h1>
            </div>
            <p style="margin-bottom: 10px; font-size: 94%">
                Blog <a href="https://yunwoong.tistory.com/">Be Original</a>
            </p>
        </div>""")
 
    with gr.Row():
        chatbot = gr.Chatbot(elem_id='chatbot')
 
    with gr.Row():
        txt = gr.Textbox(show_label=False, placeholder='Send a message...').style(container=False)
 
    txt.submit(answer, [state, state_chatbot, txt], [state, state_chatbot, chatbot])
    txt.submit(lambda: '', None, txt)
 
demo.launch(debug=True, share=True)
```

Python 스크립트를 수행합니다.

<div align="center">
<img src="https://blog.kakaocdn.net/dn/bf3RiU/btr7MVwVULe/H7uW88kBJ0OvdeqQkhDGKK/img.gif" width="70%">
</div>

------

Web 애플리케이션을 만들기 위해 새로 알아야 하는 기반 기술(Javascript, CSS 등)의 학습없이 빠르면 5분내에 Web 애플리케이션을 만들어 테스트가 가능합니다.
