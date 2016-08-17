# Enigma
## The Enigma criptographyc machine

### Intro

Simulador da máquina de (des)criptografia alemã em sua primeira versão (M1).

Elementos a considerar como chave:

* Ordem dos 3 rotores
* Posição inicial dos 3 rotores
* 6 cabos no painel frontal opcionais para trocar letras

### A entrada da configuração

#### Rotores

##### Ordem

A ordem da configuração é dada pela chave `-r` seguida de uma string indicando quais rotores usar. Exemplo:

```
$./ex5.x -r 1,3,2
```

No exemplo, o rotor mais à _direita_ (que gira mais rápido) será o rotor `I` (1); o rotor central será o rotor `III` (3) e o rotor mais à esquerda (o mais lento) será o rotor `II` (2).

A chave `-r` é obrigatória.

##### Posição inicial

A posição inicial dos rotores é dada por uma _string_ de 3 letras indicando os rotores da direita para a esquerda. Exemplo:


```
$./ex5.x -r 1,3,2 -i abc
```

O rotor mais a direita (rotor 1) é inicializado com `a`, o rotor central (rotor 3) com `b` e o rotor mais à esquerda (rotor 2) é inicializado com `c` no exemplo acima.

A chave `-i` é opcional e se não for incluída será considerada a inicialização `aaa`.

##### Configuração da fiação

Os 3 rotores originais são configurados conforme a seguinte tabela que marca a ligação dos fios internamente:

Rotor | ABCDEFGHIJKLMNOPQRSTUVWXYZ | Gira em
------|----------------------------|--------
I     | EKMFLGDQVZNTOWYHXUSPAIBRCJ |  Q
II    | AJDKSIRUXBLHWTMCQGZNPYFVOE |  E
III   | BDFHJLCPRTXVZNYEIWGAKMUSQO |  V
IV    | ESOVPZJAYQUIRHXLNFTGKDCMWB |  J
V     | VZBRGITYUPSDNHLXAWMJQOFECK |  Z
UKW-A | EJMZALYXVBWFCRQUONTSPIKHGD | fix
UKW-B | YRUHQSLDPXNGOKMIEBFZCWVJAT | fix
UKW-C | FVPJIAOYEDRZXWGCTKUQSBNMHL | fix

Neste simulador simplificado, apenas os rotores I, II e III serão usados, e o refletor UKW-B que é fixo, localizado na extrema direita. O refletor UKW-A foi usado apenas antes da Segunda Guerra, sendo o UKW-B o refletor padrão utilizado por toda a guerra. O refletor UKW-C foi introduzido apenas ao final da guerra e foi pouco utilizado.

#### Painel frontal

O painel frontal possui _jacks_ para todas as letras, e o Enigma na versão inicial acompanhava 6 cabos; isto permitia que se trocasse (opcionalmente) até no máximo 6 pares de letras do alfabeto.

A configuração do programa se dará com a chave `-p` seguida de até 6 pares de letras separadas por vírgulas. Exemplo:

```
$./ex5.x -r 1,2,3 -i abc -p aj,kw,mr
```

No exemplo acima, os pares de letras `(a,j)`, `(k,w)` e `(m,r)` são trocados antes de se enviar o sinal para os rotores e na volta após retornar o sinal dos rotores.

A chave `-p` é opcional. Se não for dada, se considera que nenhum cabo foi inserido (não há trocas de letras). Se for dada, deve vir acompanhada de pelo menos um par de letras (mínimo) e no máximo 6 pares.

#### Teste

Exemplos

1. Teste básico:

Máquina configurada como:

```
$./ex5.x -r 1,2,3 -i zaa
```

Ou seja, posição inicial dos rotores é `1=I=z, 2=II=a, 3=III=a` e sem uso de cabos plugados no painel frontal.

* Entrada: AAAA...AA (27 letras A seguidas)
* Saída: NFTZMGISXIPJWGDNJJCOQTYRIGD

2. Teste com plugs trocando `(A,B) e (C,D)`, configuração inicial `jkl` e rotores `1,3,2`

```
$./ex5.x -r 1,2,3 -i zaa
```

Ou seja, posição inicial dos rotores é `1=I=z, 2=II=a, 3=III=a` e sem uso de cabos plugados no painel frontal.

* Entrada: AAAA...AA (27 letras A seguidas)
* Saída: NFTZMGISXIPJWGDNJJCOQTYRIGD

2. Teste com plugs trocando `(A,J), (K,W) e (M,R)`, rotores `1=I=a, 2=III=b, 3=II=c`:

```
$./ex5.x -r 1,3,2 -i abc -p aj,kw,mr
```

* Entrada: AAAAAAAAAAAAAAAAAAAAAAAAAAA (27 letras A seguidas)
* Saída:   YGTIIYGZWULIRUKZZSNUKXENFHP

3. Teste de "passo duplo" (double-step).

Cada rotor ao passar do seu ponto de girar, gira o rotor seguinte (o rotor mais à direita gira o rotor imediatamente à esquerda). Mas há um mecanismo implementado chamado "passo duplo". O rotor da direita (primeiro rotor) sempre gira uma posição a cada tecla digitada. Ao passar pelo seu ponto de girar (pino na letra correspondente) ele gira também o rotor central em uma posição. Entretanto, se o rotor central atingir o seu ponto de girar, na tecla seguinte ele irá girar de novo de imediato.

Para testar este exemplo considere a configuração e a entrada:

```
$./ex5.x -r 1,2,3 -i oda
```

O rotor 1 gira o rotor 2 ao passar pela letra Q, colocando o rotor 2 na letra E que é seu ponto de giro. Na próxima tecla, o rotor 2 gira novamente para a letra F fazendo também o rotor 3 girar para a letra B. Daí por diante estabiliza, girando só o rotor 1 (até que o ciclo recomece).    

* Entrada: AAAAAA (6 letras A seguidas)
* Saída:   HDZGOV

Configurações que passam os rotores a partir da inicial: oda -> pda -> qda -> rea -> sfb -> tfb -> ufb ...

### Funcionamento

A máquina espera uma _string_ de até 250 caracteres no máximo vindas da entrada padrão (que pode ser redirecionada pelos comandos de redirecionamento do sistema operacional, conforme documentação sobre o assunto no hydrabox).

Exemplos de execução:

```
$./ex5.x -r 1,3,2 -i abc -p aj,kw,mr
```

Lê um texto digitado pelo teclado (entrada padrão) e imprime o resultado de/criptografado na tela (saída padrão).

```
$./ex5.x -r 1,3,2 -i abc -p aj,kw,mr < tocript_msg1.txt
```

Lê o arquivo de entrada `tocript_msg1.txt` que está em texto plano e imprime na saída padrão o texto criptografado.

```
$./ex5.x -r 1,3,2 -i abc -p aj,kw,mr < tocript_msg1.txt > todecript_msg1.txt
```

Lê o arquivo de entrada `tocript_msg1.txt` que está em texto plano e grava a saída criptografada em `to_decript_msg1.txt`.

```
$./ex5.x -r 1,3,2 -i abc -p aj,kw,mr < todecript_msg1.txt
```

Lê o arquivo de entrada `todecript_msg1.txt` que está criptografado e imprime na saída padrão o texto plano.

## Referências

* Cipher Machines and Cryptology: http://users.telenet.be/d.rijmenants/en/enigmamenu.htm
* Crypto Museum: http://www.cryptomuseum.com/crypto/enigma/m3/index.htm
* University of California: https://youtu.be/ncL2Fl6prH8
* Numberphile, Video 1: https://youtu.be/G2_Q9FoD-oQ
* Numberphile, Video 2: https://youtu.be/V4V2bpZlqx8

## Fale com o autor

* Autor: Prof. Ruben Carlo Benante
* Email: rcb@upe.br
* Data: 2016-08-16
* Licensa: GNU/GPL v2.0

