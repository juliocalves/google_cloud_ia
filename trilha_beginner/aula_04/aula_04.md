# Guia de Estudo: Prompt Design e Engenharia de Prompt

Este guia é focado no estudo de **Prompt Design** e **Engenharia de Prompt**, utilizando as capacidades de **Vertex AI Generative AI** da Google Cloud Platform. Este documento explora conceitos, estratégias, e fornece exemplos práticos em Python.

## Conceitos Fundamentais

### O que é Prompt Design?

Prompt Design é o processo de formular entradas (prompts) para modelos de IA generativa de maneira que maximizem a qualidade, relevância e utilidade das respostas geradas.

### O que é Engenharia de Prompt?

Engenharia de Prompt vai além do design, incorporando métodos sistemáticos e experimentais para melhorar prompts, adaptando-os a tarefas específicas e otimizando os resultados com base nos objetivos.

---

## Estratégias para Design de Prompts

1. **Clareza:** Certifique-se de que o prompt é claro, específico e fácil de entender.
2. **Contexto:** Adicione informações contextuais para orientar o modelo.
3. **Restrição:** Use restrições para guiar o modelo em tarefas específicas.
4. **Testes Iterativos:** Experimente diferentes variações do prompt para encontrar a versão mais eficaz.
5. **Formato Estruturado:** Para tarefas que exigem respostas organizadas, use formatos como JSON ou Markdown.
6. **Exploração Multimodal:** Em tarefas multimodais (texto + imagem, vídeo, etc.), forneça dados e prompts complementares.

---

## Ferramentas para Configuração de Prompts Seguros

A documentação oficial do Vertex AI aborda como configurar **filtros de segurança** para prompts. Esses filtros ajudam a evitar respostas inadequadas ou não seguras. Leia a documentação oficial para mais informações: [Configuração de Filtros de Segurança](https://cloud.google.com/vertex-ai/generative-ai/docs/multimodal/configure-safety-filters).

### Configurando Filtros de Segurança no Vertex AI

```python
from google.cloud import aiplatform
from vertexai.language_models import GenerationConfig

# Configurar o cliente
project_id = "seu-projeto"
location = "us-central1"
model_name = "text-bison"

# Inicializar o cliente Vertex AI
aiplatform.init(project=project_id, location=location)

# Configuração do modelo com filtros de segurança
config = GenerationConfig(
    safety_settings=[
        {"category": "harm_category_unspecified", "threshold": 1},
        {"category": "violence", "threshold": 2},
        {"category": "self_harm", "threshold": 2}
    ]
)

# Exemplo de prompt
prompt = "Explique como a IA pode ser usada para criar soluções de energia renovável."

response = model.predict(
    content=prompt,
    generation_config=config
)

print(response.text)
```

---

## Exemplos de Implementação

### 1. Design de Prompts para Respostas Estruturadas

O exemplo a seguir demonstra como projetar um prompt para gerar respostas em formato JSON.

```python
from vertexai.language_models import TextGenerationModel
from vertexai.language_models import GenerationConfig

# Inicializar o modelo
model = TextGenerationModel.from_pretrained("text-bison")

# Definir o esquema de resposta
response_schema = {
    "type": "OBJECT",
    "properties": {
        "tema": {"type": "STRING"},
        "resumo": {"type": "STRING"}
    },
    "required": ["tema", "resumo"]
}

# Prompt para uma resposta estruturada
prompt = """
Gere um resumo sobre o impacto da energia solar no meio ambiente.
Retorne no seguinte formato:
{
    "tema": "<O tema do texto>",
    "resumo": "<Resumo do conteúdo.>",
}
"""

response = model.predict(
    content=prompt,
    generation_config=GenerationConfig(
        response_mime_type="application/json",
        response_schema=response_schema
    )
)

print(response.text)
```

### 2. Design Multimodal com Vídeos

Este exemplo ilustra como usar prompts para tarefas multimodais, como vídeos.

```python
from vertexai.language_models import Part
from vertexai.language_models import GenerationConfig

# Configurando o arquivo de vídeo
video_file = Part.from_uri(
    "gs://cloud-samples-data/generative-ai/video/rio_de_janeiro_beyond_the_map_rio.mp4",
    "video/mp4"
)

# Prompt para análise do vídeo
prompt = """
Analise este vídeo e divida-o em capítulos. Para cada capítulo, forneça um resumo breve dos principais eventos.
"""

response = model.generate_content(
    contents=[video_file, prompt],
    generation_config=GenerationConfig(
        response_mime_type="text/plain"
    )
)

print(response.text)
```

#### 3: Extração de Informações em Texto

```python
from vertexai.language_models import TextGenerationModel
from vertexai.language_models import GenerationConfig

model = TextGenerationModel.from_pretrained("text-bison")

prompt = """
Extraia as seguintes informações do texto abaixo:
- Nome da empresa
- Localização
- Área de atuação

Texto: "TechCorp é uma empresa situada em São Paulo que atua no ramo de tecnologia da informação."
"""

response = model.predict(
    prompt,
    generation_config=GenerationConfig(
        max_output_tokens=256,
        temperature=0.5,
    )
)

print(response.text)
```

#### 4: Geração de Capítulos para um Documento

```python
from vertexai.language_models import TextGenerationModel
from vertexai.language_models import GenerationConfig

model = TextGenerationModel.from_pretrained("text-bison")

prompt = """
Divida o texto abaixo em capítulos e forneça um título curto para cada um:

Texto:
"Durante o século XX, vários avanços tecnológicos transformaram o mundo. A chegada da eletricidade, dos computadores, e da internet revolucionaram a comunicação e os negócios..."
"""

response = model.predict(
    prompt,
    generation_config=GenerationConfig(
        max_output_tokens=256,
        temperature=0.7,
    )
)

print(response.text)
```

#### 5: Classificação de Sentimentos

```python
from vertexai.language_models import TextGenerationModel
from vertexai.language_models import GenerationConfig

model = TextGenerationModel.from_pretrained("text-bison")

prompt = """
Analise o sentimento do seguinte texto e responda com "positivo", "negativo" ou "neutro":

Texto: "Adorei o atendimento, mas achei os preços um pouco altos."
"""

response = model.predict(
    prompt,
    generation_config=GenerationConfig(
        max_output_tokens=256,
        temperature=0.2,
    )
)

print(response.text)
```

---

### Zero-shot, One-shot e Few-shot Learning

#### Zero-shot

No método zero-shot, o modelo é solicitado a realizar uma tarefa sem exemplos prévios. Esse é o tipo de abordagem mais simples.

##### Exemplo: Tradução

```python
prompt = """
Traduza a seguinte frase para o inglês:
"Eu amo aprender sobre inteligência artificial."
"""

response = model.predict(
    prompt,
    generation_config=GenerationConfig(
        max_output_tokens=256,
        temperature=0.3,
    )
)

print(response.text)
```

#### One-shot

No método one-shot, fornecemos ao modelo um exemplo antes de pedir a resposta.

##### Exemplo: Resumo de Texto

```python
prompt = """
Exemplo:
Texto: "Hoje foi um dia muito produtivo. Consegui concluir várias tarefas e ainda encontrei tempo para exercícios."
Resumo: "Dia produtivo com tarefas concluídas e tempo para exercícios."

Agora faça o resumo do seguinte texto:
"Passei a manhã toda estudando para a prova de matemática. À tarde, fiz uma caminhada no parque."
"""

response = model.predict(
    prompt,
    generation_config=GenerationConfig(
        max_output_tokens=256,
        temperature=0.3,
    )
)

print(response.text)
```

#### Few-shot

No método few-shot, fornecemos vários exemplos para ajudar o modelo a compreender o padrão desejado.

##### Exemplo: Classificação de Sentimentos

```python
prompt = """
Exemplos:
Texto: "Eu adorei o filme, foi incrível!"
Sentimento: Positivo

Texto: "O serviço foi péssimo e a comida estava fria."
Sentimento: Negativo

Texto: "O produto chegou no prazo, mas a embalagem estava amassada."
Sentimento: Neutro

Agora analise o sentimento do seguinte texto:
"Achei o lugar aconchegante e o atendimento excelente."
"""

response = model.predict(
    prompt,
    generation_config=GenerationConfig(
        max_output_tokens=256,
        temperature=0.3,
    )
)

print(response.text)
```

---

### Conclusão

Estes exemplos destacam como os métodos zero-shot, one-shot e few-shot podem ser utilizados para criar prompts eficazes em diferentes cenários. Dependendo da tarefa, adicionar exemplos (one-shot/few-shot) pode melhorar a precisão do modelo.

---

# Estudo sobre `system_instruction` no Design e Engenharia de Prompts

## Introdução ao `system_instruction`

O conceito de `system_instruction` é utilizado para fornecer ao modelo um contexto ou comportamento desejado antes de processar os comandos ou perguntas do usuário. É uma ferramenta poderosa para ajustar o tom, formato de resposta ou funcionalidade da IA em um dado cenário.

Em um sistema de aprendizado generativo, as instruções do sistema agem como um "manual" que direciona a IA, permitindo que ela se ajuste às expectativas do desenvolvedor ou do usuário final. Elas geralmente precedem os comandos ou perguntas na entrada de texto.

---

## Conceito

O `system_instruction` pode:

- Definir o comportamento geral do modelo (e.g., ser educado, responder de forma técnica ou criativa).
- Fornecer restrições ou limites (e.g., não criar informações fictícias ou seguir um formato específico).
- Estabelecer o propósito da interação (e.g., atuar como tradutor, solucionador de problemas ou planejador).

### Exemplos de instruções de sistema comuns:

1. "Você é um assistente técnico especializado em segurança cibernética."
2. "Responda de maneira formal e concisa."
3. "Forneça apenas fatos verificáveis."
4. "Você é um tutor de matemática para estudantes do ensino médio."

---

## Implementação em Python com `system_instruction`

A seguir, veja como usar o conceito de `system_instruction` com exemplos práticos em Python.

### Exemplo 1: Definindo o tom formal e técnico

```python
from vertexai.language_models import ChatModel

# Carrega o modelo de chat
chat_model = ChatModel.from_pretrained("chat-bison")

# Define a instrução do sistema
system_instruction = "Você é um assistente técnico especializado em linguagens de programação."

# Cria uma conversa com a instrução do sistema
chat = chat_model.start_chat(
    context=system_instruction,
    examples=[]
)

# Envia uma pergunta
response = chat.send_message(
    "Quais são as melhores práticas para escrever código Python eficiente?",
    temperature=0.2
)

print(response.text)
```

**Saída esperada:**

```
Para escrever código Python eficiente, siga estas práticas:
1. Use estruturas de dados apropriadas.
2. Evite loops desnecessários.
3. Utilize bibliotecas otimizadas como NumPy.
4. Escreva código legível e modular.
5. Faça profiling do código para identificar gargalos.
```

---

### Exemplo 2: Formatação Estruturada

```python
# Define a instrução do sistema
system_instruction = """
Você é um assistente responsável por criar relatórios no formato Markdown.
Todas as respostas devem ser detalhadas e bem formatadas.
"""

chat = chat_model.start_chat(
    context=system_instruction,
    examples=[]
)

# Envia uma solicitação
response = chat.send_message(
    "Crie um relatório sobre os benefícios da energia solar.",
    temperature=0.3
)

print(response.text)
```

**Saída esperada:**

```markdown
# Relatório: Benefícios da Energia Solar

## Introdução
A energia solar é uma das fontes de energia renováveis mais promissoras do mundo.

## Benefícios
1. **Sustentabilidade**: Reduz a dependência de combustíveis fósseis.
2. **Redução de Custos**: Após a instalação inicial, os custos operacionais são baixos.
3. **Impacto Ambiental**: Emissões de carbono significativamente menores.

## Conclusão
Investir em energia solar é uma solução viável para a crise energética global.
```

---

### Exemplo 3: Limitações de Respostas

```python
system_instruction = """
Você é um assistente que só fornece respostas factuais e verificáveis.
Se não souber a resposta, diga 'Não sei'.
"""

chat = chat_model.start_chat(
    context=system_instruction,
    examples=[]
)

response = chat.send_message("Qual é a velocidade da luz em m/s?", temperature=0.1)
print(response.text)

response = chat.send_message("Quem será o próximo presidente dos EUA?", temperature=0.1)
print(response.text)
```

**Saída esperada:**
```
Resposta 1: A velocidade da luz é de aproximadamente 299.792.458 metros por segundo.
Resposta 2: Não sei.
```

---

## Boas Práticas com `system_instruction`

1. **Definir um propósito claro:** Sempre tenha em mente o objetivo do sistema e comunique isso ao modelo de forma explícita.
2. **Limitar o escopo:** Para evitar respostas irrelevantes, limite o contexto às informações necessárias.
3. **Iterar e ajustar:** Teste diferentes instruções para verificar qual fornece o comportamento desejado.
4. **Combine com exemplos:** Complementar o `system_instruction` com exemplos melhora a consistência do modelo.

---

## Conclusão

O uso de `system_instruction` é uma técnica essencial para moldar o comportamento dos modelos de linguagem, garantindo resultados mais úteis, alinhados e personalizados. Ao combinar instruções claras com exemplos bem estruturados, você pode criar sistemas de IA otimizados para resolver problemas específicos e atender às necessidades dos usuários.

---

## Boas Práticas no Design de Prompts

1. **Valide os Resultados:** Sempre teste a resposta do modelo contra os critérios definidos.
2. **Refine Iterativamente:** Pequenas mudanças no prompt podem gerar melhorias significativas.
3. **Use Exemplos:** Inclua exemplos no prompt para orientar o modelo em tarefas complexas.
4. **Adote Formatos Estruturados:** Formatos como JSON, XML ou Markdown facilitam a leitura e o processamento das respostas.

---

## Próximos Passos

- Explore a documentação completa do [Vertex AI Generative AI](https://cloud.google.com/vertex-ai/docs).
- Pratique ajustando filtros de segurança e experimentando prompts em diferentes domínios.
- Use as APIs do Vertex AI para criar soluções personalizadas em suas aplicações.

---

**Referências:**

- [Vertex AI Generative AI: Configurando Filtros de Segurança](https://cloud.google.com/vertex-ai/generative-ai/docs/multimodal/configure-safety-filters)
- [Documentação Oficial Vertex AI](https://cloud.google.com/vertex-ai/docs)
