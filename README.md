# Instalação

## Passo 1: Cria um Ambiente Virtual (VENV)
`py -m venv venv`


## Passo 2:
`.\venv\Scripts\activate`

## Passo 3:
`pip install -r requirements.txt`


# Problema Escolhido

## 🌾 **Cenário 2: Agricultor - Maximizar Lucro da Colheita**

**Situação:**  
Tens 10 toneladas de feijões misturados após a colheita. O White Haricot vale **3x mais** que Mixed Beans no mercado grossista.

**O Problema:**  
Se classificares **White Haricot como Mixed** (falso negativo):
- Vendes produto premium pelo preço normal
- Perda direta de receita (€5/kg → €2/kg)
- Desperdiças o potencial da tua colheita

Se classificares **Mixed como White** (falso positivo):
- O grossista vai detetar e devolver o lote
- Perdes credibilidade

**Questão:**  
Qual métrica te ajuda a **capturar TODO o valor premium** da colheita, mesmo que tenhas de verificar manualmente alguns lotes?

**Resposta: Recall (White Haricot)**

**Porquê?**  
Recall responde: "De TODO o White Haricot real que tenho, quanto % consegui identificar?"

**Alto Recall = Não deixar escapar produto premium**
- Capturas a maior parte do feijão premium disponível
- Maximizas a receita da colheita
- Podes fazer inspeção manual nos casos duvidosos

**Estratégia inteligente:**  
Se o modelo diz que é White Haricot mas tem baixa confiança, fazes inspeção visual. Mas NUNCA queres que White Haricot real seja classificado como Mixed!


# Problema Escolhido - Prompt melhorado

1. O Problema 🌾
Um agricultor tem uma colheita que mistura dois tipos de feijão: White Haricot (um produto premium que vale 3x mais) e Mixed Beans (um produto comum).

O problema central é que os dois tipos de erros que um modelo de classificação pode cometer ao tentar separá-los têm impactos de negócio muito diferentes e desiguais.

2. Explicação do Problema
Temos de analisar as duas formas como o nosso modelo pode falhar e o que cada falha custa ao agricultor:

Erro A: O Falso Negativo (Classificar Premium como Comum)

O que acontece: O modelo olha para um feijão White Haricot (valioso) e classifica-o erradamente como Mixed Beans (barato).

Impacto no Negócio: O agricultor vende o seu melhor produto ao preço baixo. Isto é uma perda de receita direta, imediata e irrecuperável.

Erro B: O Falso Positivo (Classificar Comum como Premium)

O que acontece: O modelo olha para Mixed Beans (baratos) e classifica-os erradamente como White Haricot (valiosos).

Impacto no Negócio: O lote "premium" fica contaminado. O distribuidor deteta a mistura, recusa a encomenda e o agricultor perde credibilidade (ou, em alternativa, tem de pagar por uma inspeção manual para limpar o lote antes de o enviar).

3. O Nosso Objetivo 🎯
Ao comparar os dois erros, a perda de receita (Erro A) é o pior cenário para o negócio. A perda de credibilidade ou o custo de inspeção manual (Erro B) é um problema gestionável, mas menos grave do que perder 3x o valor do produto.

Portanto, o objetivo do nosso modelo é garantir que nenhum White Haricot verdadeiro seja deixado para trás.

Tecnicamente, isto significa que temos de Maximizar o RECALL (Sensibilidade) da classe "White Haricot".

O nosso objetivo é construir um modelo que encontre todos os feijões premium, mesmo que isso signifique "apanhar" alguns feijões comuns pelo caminho (Erro B), que podem depois ser tratados manualmente.
