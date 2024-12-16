# Roles de `system_instruction` no Design de Prompts

## Introdução às Roles de `system_instruction`

As *roles* de `system_instruction` são utilizadas para orientar o comportamento do modelo de IA. Cada role define um papel específico que o modelo deve desempenhar, influenciando como ele processa informações e responde ao usuário. Isso é crucial para ajustar o tom, formato e funcionalidade da IA, permitindo maior controle e alinhamento com as expectativas do usuário.

---

## Principais Roles no Contexto de `system_instruction`

1. **Instrutor Técnico:** O modelo é configurado para fornecer explicações detalhadas e precisas, ajudando usuários a entender conceitos complexos ou executar tarefas técnicas.
2. **Tradutor ou Mediador:** O modelo atua como tradutor, reformulador ou intérprete, convertendo informações para diferentes idiomas ou formatos.
3. **Consultor Criativo:** A IA assume um papel criativo, gerando ideias ou ajudando em tarefas de brainstorming.
4. **Avaliador ou Crítico:** O modelo avalia conteúdo fornecido pelo usuário, identificando erros, inconsistências ou oportunidades de melhoria.
5. **Assistente Focado em Dados:** A IA fornece apenas informações baseadas em fatos e estruturadas de maneira objetiva.

---

## Exemplo de Implementações para Cada Role

### 1. Instrutor Técnico

O modelo atua como um especialista técnico explicando conceitos complexos.

```python
system_instruction = """
Você é um instrutor técnico que explica conceitos de ciência da computação de maneira clara e detalhada.
Responda com exemplos práticos quando aplicável.
"""

chat = chat_model.start_chat(
    context=system_instruction,
    examples=[]
)

response = chat.send_message("Explique o conceito de recursão em programação.", temperature=0.3)
print(response.text)
```

**Saída esperada:**

```
Recursão é uma técnica de programação onde uma função chama a si mesma para resolver um problema.
Exemplo em Python:
```python
def fatorial(n):
    if n == 0:
        return 1
    return n * fatorial(n - 1)
print(fatorial(5))  # Saída: 120
```
```

---

### 2. Tradutor ou Mediador
O modelo traduz ou reformula informações.

```python
system_instruction = """
Você é um tradutor que converte textos de maneira precisa entre inglês e português.
Mantenha o significado original do texto e ajuste expressões idiomáticas para o idioma de destino.
"""

chat = chat_model.start_chat(
    context=system_instruction,
    examples=[]
)

response = chat.send_message("Translate: 'The early bird catches the worm.'", temperature=0.3)
print(response.text)
```

**Saída esperada:**

```
"O pássaro madrugador pega a minhoca."
```

---

### 3. Consultor Criativo

O modelo ajuda na geração de ideias.

```python
system_instruction = """
Você é um consultor criativo que ajuda na criação de ideias inovadoras para campanhas publicitárias.
Sugira conceitos originais e envolventes.
"""

chat = chat_model.start_chat(
    context=system_instruction,
    examples=[]
)

response = chat.send_message(
    "Crie uma ideia para uma campanha de uma nova linha de roupas sustentáveis.",
    temperature=0.7
)
print(response.text)
```

**Saída esperada:**
```
Campanha: "Vista o Futuro"
Slogan: "Sustentabilidade é o novo estilo."
Ideia: Uma série de vídeos mostrando como cada peça da coleção contribui para o meio ambiente, com QR codes que rastreiam a origem dos materiais.
```

---

### 4. Avaliador ou Crítico

O modelo revisa e avalia conteúdos.

```python
system_instruction = """
Você é um crítico literário que analisa textos fornecendo feedback construtivo.
Considere gramática, clareza, coesão e estilo.
"""

chat = chat_model.start_chat(
    context=system_instruction,
    examples=[]
)

response = chat.send_message(
    "Avalie o texto: 'O sol brilhou forte naquele dia, iluminando cada canto da floresta.'",
    temperature=0.5
)
print(response.text)
```

**Saída esperada:**
```
O texto é bem escrito e evocativo. No entanto, você poderia descrever mais os sentimentos ou o impacto da luz do sol na floresta para aumentar a imersão do leitor.
```

---

### 5. Assistente Focado em Dados

O modelo fornece respostas baseadas estritamente em fatos.

```python
system_instruction = """
Você é um assistente que responde com dados factuais e verificáveis.
Se não souber a resposta, diga 'Não sei'.
"""

chat = chat_model.start_chat(
    context=system_instruction,
    examples=[]
)

response = chat.send_message("Qual é a capital da Austrália?", temperature=0.2)
print(response.text)
```

**Saída esperada:**
```
A capital da Austrália é Canberra.
```

---

## Melhores Práticas ao Usar Roles em `system_instruction`

1. **Escolha uma role apropriada para o contexto.** Seja claro ao descrever o papel que o modelo deve desempenhar.
2. **Teste diferentes instruções.** Ajuste o texto do `system_instruction` para alinhar as respostas com suas expectativas.
3. **Combine roles com exemplos.** Fornecer exemplos no prompt ajuda o modelo a entender melhor o que é esperado.

---

## Conclusão

As *roles* de `system_instruction` permitem moldar o comportamento do modelo para diferentes finalidades, aumentando sua utilidade em aplicações específicas. Usar essas roles com clareza e propósito é essencial para maximizar o potencial da IA e oferecer uma experiência mais relevante ao usuário.

---

# Guia de Instruções de Sistema para o Modelo Gemini

As instruções de sistema são um conjunto de comandos usados para informar ao modelo como ele deve se comportar e responder. Elas permitem personalizar o comportamento do modelo, como o tom da resposta, o formato da saída e a definição de metas. Abaixo estão exemplos de como implementar instruções de sistema e um guia para elaborar suas próprias.

## Índice

1. [Definir um Perfil ou Função](#definir-um-perfil-ou-função)
2. [Definir o Formato de Saída](#definir-o-formato-de-saída)
3. [Definir Estilo e Tom da Resposta](#definir-estilo-e-tom-da-resposta)
4. [Fornecer Mais Contexto](#fornecer-mais-contexto)
5. [Instruções de Idioma](#instruções-de-idioma)
6. [Exemplos de Implementação](#exemplos-de-implementação)
7. [Guia para Criar Instruções de Sistema](#guia-para-criar-instruções-de-sistema)

---

## 1. Definir um Perfil ou Função

As instruções de sistema podem ser usadas para definir o papel do modelo, por exemplo, se ele deve atuar como assistente técnico, professor ou especialista.

### Exemplo:

```python
model = GenerativeModel(
    model_name="gemini-1.5-pro-002",
    system_instruction="You are a helpful assistant who provides concise and accurate answers to tech-related questions."
)
```

**Comando:**  
Perguntar sobre o sistema de arquivos de um sistema operacional.

**Resposta Esperada:**  
Explicação clara e técnica sobre o sistema de arquivos, sem grandes desvios.

---

## 2. Definir o Formato de Saída

Você pode usar as instruções de sistema para especificar o formato em que deseja que o modelo forneça a resposta, como Markdown, JSON ou YAML.

### Exemplo:

```python
model = GenerativeModel(
    model_name="gemini-1.5-flash-002",
    system_instruction="Please provide your responses in JSON format."
)
```

**Comando:**  
Descrever o processo de instalação de um servidor web.

**Resposta Esperada:**

```json
{
  "steps": [
    "Download the server software.",
    "Install the software using the setup wizard.",
    "Configure your server settings."
  ]
}
```

---

## 3. Definir Estilo e Tom da Resposta

Você pode controlar o estilo e o tom da resposta, como o nível de formalidade ou a quantidade de detalhes.

### Exemplo:

```python
model = GenerativeModel(
    model_name="gemini-1.5-pro-002",
    system_instruction="You are a technical expert. Please provide detailed, precise answers with an academic tone."
)
```

**Comando:**  
Explicar como funciona a programação orientada a objetos.

**Resposta Esperada:**  
Uma resposta técnica e detalhada, com exemplos de código explicativos.

---

## 4. Fornecer Mais Contexto

Use as instruções de sistema para dar ao modelo mais informações contextuais, como o cenário ou as limitações da tarefa.

### Exemplo:

```python
model = GenerativeModel(
    model_name="gemini-1.5-flash-002",
    system_instruction="You are an assistant helping with customer service. Focus on empathy and providing personalized solutions."
)
```

**Comando:**  
Responder a uma reclamação de cliente sobre um produto defeituoso.

**Resposta Esperada:**  
Uma resposta empática que ofereça uma solução para o problema, mantendo um tom amigável e atencioso.

---

## 5. Instruções de Idioma

Você pode especificar o idioma em que o modelo deve responder, especialmente útil quando os comandos não estão em inglês.

### Exemplo:

```python
model = GenerativeModel(
    model_name="gemini-1.5-pro-002",
    system_instruction="All responses should be in Portuguese, unless specified otherwise."
)
```

**Comando:**  
Perguntar como configurar um servidor de e-mail.

**Resposta Esperada:**  
A explicação será fornecida em português.

---

## 6. Exemplos de Implementação

Aqui estão exemplos de como você pode utilizar as instruções de sistema em diferentes contextos:

### Geração de Códigos

**Instrução de Sistema:**

```
You are a coding expert. When I describe a component, return the HTML and CSS code necessary to implement it.
```

**Comando:**
```
Create a login form with fields for username, password, and a submit button.
```

**Resposta Esperada:**

```html
<form>
  <label for="username">Username:</label>
  <input type="text" id="username" name="username">
  <label for="password">Password:</label>
  <input type="password" id="password" name="password">
  <button type="submit">Submit</button>
</form>
```

### Análise de Sentimento do Mercado

**Instrução de Sistema:**

```
You are a stock market analyst. Extract statements that impact investor sentiment from the provided news snippet.
```

**Comando:**

```
Mobileye forecasts a significant drop in revenue for Q1 2024 due to excess inventory.
```

**Resposta Esperada:**

```json
{
  "sentiment_score": 3,
  "statement": "Mobileye forecasts a significant drop in revenue for Q1 2024 due to excess inventory.",
  "explanation": "This is a negative sentiment, as it signals financial challenges for the company."
}
```

---

## 7. Guia para Criar Instruções de Sistema

### Passos para Criar Instruções Eficientes:

1. **Seja Claro e Específico:**  
   Defina claramente o que o modelo deve fazer. Evite ambiguidade.

2. **Descreva o Papel do Modelo:**  
   Se o modelo tiver uma função específica, como assistente ou consultor, especifique isso claramente.

3. **Defina Expectativas de Formatação e Estilo:**  
   Se a saída precisa estar em um formato específico, como JSON, YAML ou Markdown, defina isso nas instruções.

4. **Considere o Contexto:**  
   Se você tem um cenário específico ou condições de tarefa, forneça o contexto necessário.

5. **Use Linguagem Natural e Objetiva:**  
   As instruções de sistema devem ser facilmente compreendidas. Evite jargões ou linguagem excessivamente técnica, a menos que o modelo precise atuar em um campo especializado.

---

## Conclusão

As instruções de sistema são uma poderosa ferramenta para personalizar o comportamento do modelo Gemini. Elas permitem um controle granular sobre como o modelo responde, adaptando-o para diferentes tarefas e cenários. Usando as dicas e exemplos fornecidos, você pode criar instruções próprias para atender às suas necessidades específicas.

Esse arquivo inclui instruções para diferentes tipos de uso e como elaborar suas próprias, permitindo personalizar as respostas e o comportamento do modelo conforme suas necessidades