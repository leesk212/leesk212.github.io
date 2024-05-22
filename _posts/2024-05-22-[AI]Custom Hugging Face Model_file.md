---
toc: True
tags: AI Llama3 HuggingFace
---


```txt
FROM llama-3-Korean-Bllossom-8B-Q4_K_M.gguf

TEMPLATE """{{- if .System }}
<s>{{ .System }}</s>
{{- end }}
<s>Human:
{{ .Prompt }}</s>
<s>Assistant:
"""

SYSTEM """A chat between a curious user and an artificial intelligence assistant. The assistant gives helpful, detailed, and polite answers to the user's questions. 모든 대답은 한국어(Korean)으로 대답해줘."""

PARAMETER temperature 0
PARAMETER num_predict 3000
PARAMETER num_ctx 4096
PARAMETER stop <s>
PARAMETER stop </s>
```

PARAMTER의 경우 HuggingFaceTextGenInference API를 참고하자 

<ref : https://github.com/ollama/ollama/blob/main/docs/modelfile.md#parameter>

참조
