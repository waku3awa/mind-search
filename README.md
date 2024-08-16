# mind-serach

container-baseをVisual Studio Codeで開く。<br>
.devcontainer.jsonの設定を書く。<br>
Devcontainerをビルドする。<br>

１つ目のターミナル<br>
```bash
cd MindSearch
pip install -r requirements.txt
python -m mindsearch.app --lang jp --model_format gpt4
```
--model_format internlm_serverとすると、モデルのダウンロードが始まるので不用意にやらないこと。<br>
--model_format gpt4とするとgpt4-o-miniを使う設定になっている。
変えるときはMindSearch\mindsearch\agent\models.py L39を書き換える。

２つ目のターミナル<br>
nodejsとnpmのインストールはDockerfileでやっているので手動での対応不要
```bash
cd MindSearch/frontend/React
npm start
```

３つ目のターミナル<br>
- Gradio

```bash
cd MindSearch
python frontend/mindsearch_gradio.py
```

- Streamlit

```bash
cd MindSearch
streamlit run frontend/mindsearch_streamlit.py
```