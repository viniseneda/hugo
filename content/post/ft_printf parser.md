---
title: "Ft_printf (Parser)"
date: 2021-04-07T21:49:33-03:00
draft: false
---

O primeiro passo para resolver o problema foi o desenvolvivemento de uma função que conseguisse ler o intervalo da string entre "%" e o caracter marcador da conversão. E então identificar as informações referentes as flags e a precisão do número em questão.

O impecilho é que de acordo com a implementação do printf que temos que reproduzir, a repetição de flags ou chamadas de precisão (o '.'), não só não são ignoradas como são processadas uma depois da outra. 

A minha solução foi implementar funções recursivas que ficam atentas para os estados limites de cada flag ou das chamadas de precisão, chamando a sí próprias caso o próximo caracter ainda seja do mesmo "tipo" do anterior.

Por exemplo, a sequência:
```
%---0-0-0-0-065.43.67.54d
```
vai identificar a presença da flag '-' e da flag '0', mesmo elas se repetindo várias vezes. Depois identifica com sucesso o tamanho mínimo de caracteres ocupados pelo número como 65, e a precisão termina sendo o número que aparece depois do último '.', no caso, o 54.

São três funções que foram criadas:

1. Uma que verifica se o caracter é '0' ou '-', e vai continuar lendo a string enquando essa condição for satisfeita.

2. Uma função que determina o número minimo de caracteres ocupados, ela é capaz de ler o número na string e salvar ele como int (ou ler uma wildcard '*' da lista variável de argumentos da função).

3. A função que lê o primeiro número depois do ponto e salva ele como a precision. Justamente, se houver mais de um ponto ele vai modificando a precision conforme for avançando na leitura. Essa função também pode chamar a função 2 caso tenha uma sequência de wildcards depois de um ponto (um comportamento observado na execução do printf no sistema Mac).

Para minha surpresa essa lógica parece ter funcionado muito bem. Nos breves testes que fiz ela identificou com sucesso pelo menos os casos mais básicos. Vamos ver como ela se sustenta agora que estou programando as funções para imprimir cada uma das conversões obrigatórias pro exercício. 
