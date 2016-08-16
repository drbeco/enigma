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

#### Painel frontal

    O painel frontal possui _jacks_ para todas as letras, e o Enigma na versão inicial acompanhava 6 cabos; isto permitia que se trocasse (opcionalmente) até no máximo 6 pares de letras do alfabeto.

    A configuração do programa se dará com a chave `-p` seguida de até 6 pares de letras separadas por vírgulas. Exemplo:

    ```
    $./ex5.x -r 1,2,3 -i abc -p aj,kw,mr
    ```

    No exemplo acima, os pares de letras `(a,j)`, `(k,w)` e `(m,r)` são trocados antes de se enviar o sinal para os rotores e na volta após retornar o sinal dos rotores.

    A chave `-p` é opcional. Se não for dada, se considera que nenhum cabo foi inserido (não há trocas de letras). Se for dada, deve vir acompanhada de pelo menos um par de letras (mínimo) e no máximo 6 pares.

### Funcionamento

    A máquina espera uma _string_ de até 250 caracteres no máximo vindas da entrada padrão (que pode ser redirecionada pelos comandos de redirecionamento do sistema operacional, conforme documentação sobre o assunto no hydrabox).

    Exemplos de execução:

    ```
    $./ex5.x -r 1,2,3 -i abc -p aj,kw,mr
    ```

    Lê um texto digitado pelo teclado (entrada padrão) e imprime o resultado de/criptografado na tela (saída padrão).

    ```
    $./ex5.x -r 1,2,3 -i abc -p aj,kw,mr < tocript_msg1.txt
    ```

    Lê o arquivo de entrada tocript_msg1.txt que está em texto plano e imprime na saída padrão o texto criptografado.

    ```
    $./ex5.x -r 1,2,3 -i abc -p aj,kw,mr < tocript_msg1.txt > todecript_msg1.txt
    ```

    Lê o arquivo de entrada tocript_msg1.txt que está em texto plano e grava a saída criptografada em to_decript_msg1.txt.

    ```
    $./ex5.x -r 1,2,3 -i abc -p aj,kw,mr < todecript_msg1.txt
    ```

    Lê o arquivo de entrada todecript_msg1.txt que está criptografado e imprime na saída padrão o texto plano.

## Fale com o autor

Autor: Prof. Ruben Carlo Benante
Email: rcb@upe.br
Data: 2016-08-16
Licensa: GNU/GPL v2.0

