---
title: "Get_Next_Line"
date: 2021-03-15T18:53:21-03:00
draft: True
---

O GNL é uma função que deve retornar uma string com uma linha de um arquivo cada vez que for chamada. Ela faz isso até o "end of file", lendo um número n (que é definido como uma constante chamada BUFFER_SIZE) de bytes de cada vez e verificando se alguma condição de fim de linha -- EOF ou o \n -- existe dentro desses bytes que foram lidos. 

Nesse projeto é preciso usar as variáveis estáticas para lidar com a necessidade de interromper a leitura de um buffer entre linhas diferentes. Também é preciso compreender como o programa interage com o input de dados através dos file descriptors (fd).

Me empolguei para fazer o exercício pensando numa lógica de recursão, algo como:

```
static char	*get(int fd)
{
	beg = cursor;
	buffer = read_buffer(cursor, fd);
	while (cursor <= BUFFER_SIZE)
	{
		if(*(buffer + cursor) == '\n')
		{
			cursor++;
			return (ft_substr(buffer, beg, cursor - beg - 1));
		}
		cursor++;
	}
	current_string = ft_substr(buffer, beg, cursor - beg);	
	rest_of_line = get(fd, flag);
	cursor = 0;
	return (append(current_string, rest_of_line));
}
```

Que simplesmente iria empilhando na stack strings de tamanho BUFFER_SIZE até chegar no fim da linha, momento em que iria "descer" a stack, juntando as strings com a função append.

A princípio funcionou, mas essa ideia mais simples não da conta de dizer quando chegamos ao fim do arquivo, ou de lidar com as situações em que não há quebra de linha.

Se tornou então um desafio muito grande pensar uma forma de transmitir os "estados" da função através da recursão. A solução que encontrei foi enviar como argumento da recursão ponteiros de variáveis, que sirvam como bandeira dos estados especiais que podem ser encontrados. Como o fim do arquivo ou as linhas que não tem \n no final.

``` 
char	*get(int fd, ssize_t *status, int *flag)
```

status indicando o return da função read, e flag que indica o fim do arquivo.

Também vieram vários outros probleminhas na tentiva de implementar essa ideia, mas por hora parece estar funcionando. Devo submeter o projeto para correção logo mais.
