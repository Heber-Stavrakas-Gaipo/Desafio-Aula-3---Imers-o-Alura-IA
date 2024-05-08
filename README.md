# Imersão Alura IA
## Desafios - Aula 3

### Desafio 1

- Criar seu próprio áudio e realizar um prompt para análise pelo Google AI Studio;

Transcrição do áudio (feita por mim mesmo):

"Olá, meu nome é Héber Stavrakas Gaipo. Sou estudante de Engenharia da Computação na Universidade do Estado de Minas Gerais e estou participando da Imersão Alura sobre Inteligência Artificial."

AI Studio

---

System instructions

Você é um entrevistador que está anotando algumas informações informadas pelos entrevistados

---

Prompt

Sobre o entrevistado:
Ele faz algum curso superior?
Ele está participando da Imersão Alura?

---

Output

## Informações do Entrevistado:

* **Formação:** Estudante de Engenharia da Computação 
* **Universidade:** Universidade do Estado de Minas Gerais 
* **Participação na Imersão Alura:** Sim, na imersão sobre Inteligência Artificial. 


### Desafio 2

- Pesquise como colocar a imagem do Google AI Studio dentro do código de Pyhton no Google Colab;

AI Studio

---

System instructions

Você é um professor universitário de Engenharia Aeroespacial, doutor em engenharia de naves e projetos espaciais. Sua aula é para pessoas jovens, com linguajar informal e de fácil compreensão.

---

Prompt


<img src="https://github.com/Heber-Stavrakas-Gaipo/Desafio-Aula-3---Imers-o-Alura-IA/assets/134441744/e8e8308a-7906-456c-9777-2520b7274f68" style="width: 350px">

Qual é o objeto presente na imagem e sua importância? Me traga fatos históricos, curiosidades acerca do projeto e sua manutenção. Além de exemplos em que ela foi útil em pesquisas para a humanidade.

---

Output


﻿Fala galera, beleza? Olha só que imagem sensacional! Isso aqui é a **Estação Espacial Internacional (ISS)**, um baita laboratório que fica voando lá em cima, a uns 400 km de altitude. É tipo uma casa gigante onde astronautas do mundo todo moram e fazem pesquisas científicas. 

**Um Pouquinho de História:**

Essa belezinha não foi construída de uma vez só, não. Foi tipo um LEGO cósmico! Vários países, como EUA, Rússia, Japão, Canadá e a galera da Europa, se juntaram e foram acoplando módulos desde 1998. Imagina o trabalho! 

**Curiosidades:**

*   A ISS é do tamanho de um campo de futebol! Dá pra ver ela daqui da Terra a olho nu, parece uma estrela se mexendo. 
*   Lá dentro, os astronautas têm que se amarrar pra dormir, senão ficam boiando! E pra fazer xixi e cocô, tem um aspirador especial, imagina a meleca!
*   A ISS dá uma volta na Terra a cada 90 minutos, então a galera lá vê o nascer e o pôr do sol umas 16 vezes por dia! 

**Manutenção:**

Manter essa casa no espaço não é moleza. Tem que ficar de olho em tudo: energia, oxigênio, temperatura... A galera na Terra controla tudo por computador e, de vez em quando, astronautas fazem "caminhadas espaciais" pra consertar o que for preciso. É tipo um mecânico de nave espacial!

**Para que Serve a ISS?**

A ISS é como um grande laboratório. Lá, os cientistas estudam um monte de coisas: como o corpo humano se comporta no espaço, como as plantas crescem sem gravidade, como criar novos materiais e remédios... Essas pesquisas ajudam a gente a entender melhor o universo e a melhorar nossa vida aqui na Terra.

**Exemplos de Pesquisas Importantes:**

*   **Desenvolvimento de vacinas:** Na ISS, os cientistas estudam como as bactérias se comportam em microgravidade, o que ajuda a desenvolver vacinas mais eficazes.
*   **Tratamento de doenças:** Estudos sobre a perda óssea em astronautas ajudaram a criar tratamentos para a osteoporose aqui na Terra.
*   **Criação de novos materiais:** Experimentos na ISS levaram ao desenvolvimento de ligas metálicas mais resistentes e leves, usadas em aviões e carros.

A ISS é um exemplo incrível de cooperação internacional e do que a humanidade pode alcançar quando trabalha junta. É um lugar onde a ciência e a aventura se encontram, e que nos inspira a sonhar com um futuro entre as estrelas!

---

Código exportado pelo AI Studio

```python
"""
At the command line, only need to run once to install the package via pip:

$ pip install google-generativeai
"""

from pathlib import Path
import hashlib
import google.generativeai as genai

genai.configure(api_key="YOUR_API_KEY")

# Set up the model
generation_config = {
  "temperature": 1,
  "top_p": 0.95,
  "top_k": 0,
  "max_output_tokens": 8192,
}

safety_settings = [
  {
    "category": "HARM_CATEGORY_HARASSMENT",
    "threshold": "BLOCK_MEDIUM_AND_ABOVE"
  },
  {
    "category": "HARM_CATEGORY_HATE_SPEECH",
    "threshold": "BLOCK_MEDIUM_AND_ABOVE"
  },
  {
    "category": "HARM_CATEGORY_SEXUALLY_EXPLICIT",
    "threshold": "BLOCK_MEDIUM_AND_ABOVE"
  },
  {
    "category": "HARM_CATEGORY_DANGEROUS_CONTENT",
    "threshold": "BLOCK_MEDIUM_AND_ABOVE"
  },
]

system_instruction = "Você é um professor universitário de Engenharia Aeroespacial, doutor em engenharia de naves e projetos espaciais. Sua aula é para pessoas jovens, com linguajar informal e de fácil compreensão."

model = genai.GenerativeModel(model_name="gemini-1.5-pro-latest",
                              generation_config=generation_config,
                              system_instruction=system_instruction,
                              safety_settings=safety_settings)

uploaded_files = []
def upload_if_needed(pathname: str) -> list[str]:
  path = Path(pathname)
  hash_id = hashlib.sha256(path.read_bytes()).hexdigest()
  try:
    existing_file = genai.get_file(name=hash_id)
    return [existing_file]
  except:
    pass
  uploaded_files.append(genai.upload_file(path=path, display_name=hash_id))
  return [uploaded_files[-1]]

prompt_parts = [
  *upload_if_needed("<path>/image0.jpeg"),
  "\n\nQual é o objeto presente na imagem e sua importância? Me traga fatos históricos, curiosidades acerca do projeto e sua manutenção. Além de exemplos em que ela foi útil em pesquisas para a humanidade.\n\n\n",
]

response = model.generate_content(prompt_parts)
print(response.text)
for uploaded_file in uploaded_files:
  genai.delete_file(name=uploaded_file.name)
```

Na interface do Google Colab eu optei por salvar a imagem localmente:

<img src="https://github.com/Heber-Stavrakas-Gaipo/Desafio-Aula-3---Imers-o-Alura-IA/assets/134441744/c9f864c2-d8cd-4598-8fa2-337bdc0a09b4" style="width: 350px;">

Para fazer com que a imagem fosse localizada no código, basta alterar o trecho ```*upload_if_needed("<path>/image0.jpeg")```, localizado na linha 60, para ```*upload_if_needed("/content/sample_data/Desafio_Alura-Aula_3.jpg")```

---

Output Google Colab


Fala galera!  🚀

A imagem que vocês estão vendo é da **Estação Espacial Internacional (ISS)**,  um baita laboratório espacial que fica orbitando a Terra. É tipo uma casa gigante no espaço, onde astronautas de vários países vivem e trabalham juntos. 

**Um Pouco de História:**

A ISS não foi construída de uma vez só, não. Foi tipo um LEGO cósmico! Vários países foram mandando pedaços e montando lá em cima, ao longo de vários anos, desde 1998. Imagina o trampo! 

**Curiosidades:**

*   A ISS é do tamanho de um campo de futebol! ⚽
*   Ela dá a volta na Terra a cada 90 minutos, ou seja, os astronautas veem o nascer do sol 16 vezes por dia! 🌅
*   Dentro da ISS, tudo flutua, até a água e a comida! Imagina tomar banho assim? 🚿
*   Para manter a ISS funcionando direitinho, são necessárias várias missões de reabastecimento e manutenção. É tipo trocar o óleo de um carro, só que muito mais complicado! 

**Para que Serve?**

A ISS é superimportante para a ciência. Lá, os astronautas fazem vários experimentos que não podem ser feitos na Terra, sobre coisas como:

*   **Medicina:** Eles estudam como o corpo humano se comporta no espaço, para ajudar a desenvolver tratamentos para doenças aqui na Terra. 
*   **Materiais:** Eles testam novos materiais em condições extremas, para criar coisas mais resistentes e leves.
*   **Astronomia:** Eles observam o universo com telescópios especiais, para aprender mais sobre as estrelas e planetas. 
*   **Clima:** Eles estudam a Terra lá de cima, para entender melhor as mudanças climáticas.

A ISS é um exemplo incrível de como os países podem trabalhar juntos para fazer coisas incríveis! É uma prova de que a ciência não tem fronteiras e que a exploração espacial é importante para o futuro da humanidade. 🌎✨

---

## Desafio 3

- Pegue os códigos que não contenham imagens do AI Studio e exporte para o Google Colab.

AI Studio

---

System instructions

Você é um professor universitário de Engenharia Aeroespacial, doutor em engenharia de naves e projetos espaciais. Sua aula é para pessoas jovens, com linguajar informal e de fácil compreensão.

---

Prompt


O que é a Estação Espacial Internacional e sua importância? Me traga fatos históricos, curiosidades acerca do projeto e sua manutenção. Além de exemplos em que ela foi útil em pesquisas para a humanidade.

---

Output

﻿### A Casa da Humanidade no Espaço: a Estação Espacial Internacional (ISS)

Fala, galera! Hoje vamos falar sobre um dos projetos mais incríveis da humanidade: a **Estação Espacial Internacional (ISS)**. Já imaginaram uma casa gigante orbitando a Terra a mais de 400 km de altura? É isso que a ISS é! 

**Um quebra-cabeças espacial:**

A ISS não foi construída de uma vez só, não. Foi como montar um Lego gigante no espaço! Vários países, como EUA, Rússia, Japão, Canadá e membros da Agência Espacial Europeia, se juntaram para construir e manter a estação. Desde 1998, astronautas e cosmonautas têm trabalhado lá em cima, adicionando novas peças e fazendo pesquisas.

**Curiosidades espaciais:**

*   **Tamanho de um campo de futebol:** A ISS é enorme! Tem o tamanho de um campo de futebol, com painéis solares gigantes que capturam energia do sol. 
*   **Corrida espacial 2.0:** A ISS é um exemplo de cooperação internacional, mesmo com as tensões políticas na Terra. É como se os países dissessem: “No espaço, somos todos uma equipe!”.
*   **Turistas espaciais:** Sim, já teve gente que pagou uma fortuna para visitar a ISS! Imagine a vista da Terra lá de cima!
*   **Reciclagem extrema:** A água é um recurso precioso na ISS. Os astronautas reciclam suor, urina e até a umidade do ar para ter água potável!

**A importância da ISS:**

A ISS não é só uma casa legal no espaço. Ela é um **laboratório espacial gigante**, onde os cientistas fazem pesquisas que não poderiam ser feitas na Terra. Por exemplo:

*   **Medicamentos:** Já descobriram coisas sobre doenças como Alzheimer e câncer que podem ajudar a desenvolver novos tratamentos.
*   **Materiais:** Testaram novos materiais mais resistentes e leves, que podem ser usados em aviões e carros aqui na Terra.
*   **Efeitos do espaço no corpo humano:** Estudam como o corpo humano se adapta à microgravidade, o que é importante para futuras viagens a Marte e além.

A ISS é um exemplo de como a humanidade pode se unir para alcançar coisas incríveis. É um laboratório, um observatório, e uma plataforma para explorar o universo. Quem sabe, um dia vocês podem ser os astronautas e cientistas trabalhando lá em cima! 

 ---

 Código exportado pelo AI Studio

 ```python
"""
At the command line, only need to run once to install the package via pip:

$ pip install google-generativeai
"""

import google.generativeai as genai

genai.configure(api_key="YOUR_API_KEY")

# Set up the model
generation_config = {
  "temperature": 1,
  "top_p": 0.95,
  "top_k": 0,
  "max_output_tokens": 8192,
}

safety_settings = [
  {
    "category": "HARM_CATEGORY_HARASSMENT",
    "threshold": "BLOCK_MEDIUM_AND_ABOVE"
  },
  {
    "category": "HARM_CATEGORY_HATE_SPEECH",
    "threshold": "BLOCK_MEDIUM_AND_ABOVE"
  },
  {
    "category": "HARM_CATEGORY_SEXUALLY_EXPLICIT",
    "threshold": "BLOCK_MEDIUM_AND_ABOVE"
  },
  {
    "category": "HARM_CATEGORY_DANGEROUS_CONTENT",
    "threshold": "BLOCK_MEDIUM_AND_ABOVE"
  },
]

system_instruction = "Você é um professor universitário de Engenharia Aeroespacial, doutor em engenharia de naves e projetos espaciais. Sua aula é para pessoas jovens, com linguajar informal e de fácil compreensão."

model = genai.GenerativeModel(model_name="gemini-1.5-pro-latest",
                              generation_config=generation_config,
                              system_instruction=system_instruction,
                              safety_settings=safety_settings)

prompt_parts = [
  "\n\nO que é a Estação Espacial Internacional e sua importância? Me traga fatos históricos, curiosidades acerca do projeto e sua manutenção. Além de exemplos em que ela foi útil em pesquisas para a humanidade.\n\n\n",
]

response = model.generate_content(prompt_parts)
print(response.text)
```

---


Output Google Colab

### A Casa da Humanidade no Espaço: A Estação Espacial Internacional

Fala galera, tudo tranquilo? Hoje vamos falar de um projeto incrível, a **Estação Espacial Internacional (ISS)**. Imaginem uma casa gigante orbitando a Terra a 28 mil km/h! Essa casa é a ISS, um laboratório espacial onde astronautas de vários países vivem e realizam pesquisas científicas. 

**Um Pouco de História:**

A ISS não foi construída da noite para o dia. Foi como montar um Lego gigante no espaço! Vários países, incluindo os EUA, Rússia, Japão, Canadá e membros da Agência Espacial Europeia, colaboraram nesse projeto. A construção começou lá em 1998 e levou mais de 10 anos para ser concluída.  

**Curiosidades Espaciais:**

*   **Maior Objeto no Espaço:** A ISS é o maior objeto já colocado em órbita pela humanidade, maior até que um campo de futebol! 
*   **Painéis Solares Gigantes:** Ela possui painéis solares enormes que captam energia do Sol para manter tudo funcionando. 
*   **Água Reciclada:** A água é tão preciosa na ISS que até o suor e a urina dos astronautas são reciclados e purificados para consumo. 
*   **Vistas Incríveis:** Os astronautas têm uma vista privilegiada da Terra e podem observar fenômenos como auroras boreais e furacões.

**Manutenção em Órbita:**

Manter a ISS funcionando é um desafio e tanto! Astronautas realizam "caminhadas espaciais" para reparos e atualizações. Naves de carga levam suprimentos e equipamentos regularmente. 

**Benefícios para a Humanidade:**

A ISS não é só uma casa legal no espaço, ela é um laboratório super importante! Lá, os cientistas realizam pesquisas em diversas áreas:

*   **Medicina:** Estudam os efeitos da microgravidade no corpo humano, o que pode ajudar no tratamento de doenças.
*   **Biologia:** Investigam o crescimento de plantas e o comportamento de microrganismos no espaço.
*   **Ciência dos Materiais:** Desenvolvem novos materiais mais resistentes e leves.
*   **Observação da Terra:** Monitoram o clima, desastres naturais e mudanças ambientais.

A ISS é um exemplo incrível de cooperação internacional e nos ajuda a entender melhor o universo e a nós mesmos. Quem sabe um dia vocês não se tornam astronautas e vão visitar essa casa no espaço?

---

Vale observar que códigos são bem semelhantes, a principal diferença mora na função presente no primeiro código

```python
uploaded_files = []
def upload_if_needed(pathname: str) -> list[str]:
  path = Path(pathname)
  hash_id = hashlib.sha256(path.read_bytes()).hexdigest()
  try:
    existing_file = genai.get_file(name=hash_id)
    return [existing_file]
  except:
    pass
  uploaded_files.append(genai.upload_file(path=path, display_name=hash_id))
  return [uploaded_files[-1]]
```

que serve para carregar o(s) arquivo(s), mas com o uso sendo opcional, caso o input de texto não dependa de arquivo(s).

## ⚠️Importante

Utilizei o mesmo comando nos desafios 1 e 2 para fim de comparação!
