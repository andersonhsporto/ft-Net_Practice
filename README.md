# Net_Practice

## _A system administration exercise._

## Conceitos:

### Endereço de IP

> Endereço IP é um [endereço exclusivo] que identifica um dispositivo na Internet ou
> em uma rede local. IP vem do inglês "Internet Protocol" (protocolo de rede) que
> consiste em um conjunto de regras que regem o formato de dados enviados pela
> Internet ou por uma rede local.

Um endereço IP tem [duas partes]: o ID da rede, que compreende os três primeiros números do endereço e um ID do host, o quarto número no endereço. Então, na sua rede doméstica, que pode ser conhecida como 192.168.1.1 – 192.168.1 é o ID da rede e o .1, .2, .3, etc. final é o ID do host.

Toda endereço IP tem uma [máscara correspondente]. Essa máscara que identifica qual parte do endereço pertence a rede e qual parte ao host.

### Endereço público vs Endereço privado

> Todos os endereços IP IPv4 podem ser divididos em dois grupos principais:
> global, ou público, ou externo – este grupo também pode ser chamado de ‘endereços
> de WAN’ – aqueles que são usados na Internet, e endereços privados, locais ou
> internos – aqueles que são usados na rede local (LAN).

Um endereço IP público, também chamado de endereço IP externo ou global, é usado para a comunicação entre os hosts e a Internet global. O endereço IP público, geralmente fornecido para uso doméstico pelo seu ISP, é o que realmente conecta você à Internet. 

Os endereços internos [privados] não são roteados na Internet e nenhum tráfego pode ser enviado a eles da Internet, eles apenas deveriam funcionar dentro da rede local. Estes são destinados ao uso em redes locais fechadas e a alocação de tais endereços não é controlada globalmente por ninguém.

Os endereços privados incluem endereços IP das seguintes sub-redes:

- Faixa de 10.0.0.0 a 10.255.255.255 – uma rede 10.0.0.0 com uma máscara 255.0.0.0 ou /8 (8 bits);
- Faixa de 172.16.0.0 a 172.31.255.255 – uma rede 172.16.0.0 com uma máscara 255.240.0.0 ou /12 (12 bits);
- Faixa de 192.168.0.0 a 192.168.255.255, que é uma rede 192.168.0.0 mascarada por 255.255.0.0 ou /16 (16 bits);
- Faixa Especial de 100.64.0.0 a 100.127.255.255 com uma máscara de rede 255.192.0.0 ou /10; esta sub-rede é recomendada de acordo com a RFC6598 para uso como um pool de endereços para CGNAT (Carrier-Grade NAT).

### Máscara de sub-rede

> As [sub-redes] são utilizadas principalmente para melhorar o tráfego em cenários onde há dois ou mais computadores que estão conectados na mesma rede.
> Para isso, é feito uma divisão da rede em partes consideradas menores, e cada uma dessas partes pode representar uma área, departamento, andar, entre outros.

As partes do endereço IP recebem o nome de octetos, pois são formadas por 8 bits cada uma e cada bit terá um valor decimal que corresponde a sua posição. Fácil, né?

Como são bits, eles possuem apenas dois estados, sendo 0, que representa o host, e o 1, que representa a rede.

A máscara que usamos como exemplo (255.255.255.0) poderia ser representada em número binário como: 11111111.11111111.11111111.0000000.

### Notação CIDR

O [CIDR] possui uma notação que é extremamente compacta e que identifica o endereço IP e qual o seu prefixo de roteamento que está associado.

Sua notação é construída através de um endereço IP, uma barra (/) e, por último, um número decimal.

![CIDR_IMG](https://miro.medium.com/max/4448/1*By1Z1u0xilCm5OAtOqm3pg.png)


### Calculando o endereço de rede

Uma forma de calcular o endereço de rede com a seguinte configuração:
```
    Endereço de IP      | 104.198.241.125
    Máscara de sub-rede | 255.255.255.128  
    Notação CIDR        | /25 
```
Para determinar qual parte do endereço de ip é o endereço de rede, precisamos converter a máscara de sub-rede para notação binaria.
```
    Máscara de sub-rede | 11111111.11111111.11111111.10000000
```
Os bits da máscara que são 1 representam o endereço de rede, enquanto que os bits que são 0 representam o endereço do host.

Agora é necessario converter o endereço de ip para notação binaria.
```
    Endereço de IP      | 01101000.11000110.11110001.01111101
    Máscara de sub-rede | 11111111.11111111.11111111.10000000
```

Desta forma podemos utilizar a operação binaria [AND](https://en.wikipedia.org/wiki/Bitwise_operation#AND) para encontrar o endereço de rede do IP.
```
    Endereço de IP      | 01101000.11000110.11110001.01111101
    Máscara de sub-rede | 11111111.11111111.11111111.10000000
    Endereço de rede    | 01101000.11000110.11110001.00000000
```
Que corresponde ao endereço de rede: ``104.198.241.0``.

### Faixa de endereços de IP

Para determinar quais endereços de ip podemos utilizar em nossa rede, temos que utilizar os bit reservados para o endereço de host.

Exemplo:
```
    Endereço de IP      | 01101000.11000110.11110001.01111101
    Máscara de sub-rede | 11111111.11111111.11111111.10000000
```
A faixa de endereços de host é determinada pelos ultimos bits que são 0 dentro da máscara de sub-rede.

Na mascára ``255.255.255.128`` os ultimos 7 bits são iguais a 0.
```
    Binario | 0000000 - 1111111
    Decimal | 0 - 127
```
Para determinar qual a faixa de endereços de IP, é necessario adicionar o endereço de rede ao final da faixa de endereços de ip. 
A faixa de endereços no exemplo acima seria a ``104.198.241.0 - 104.198.241.127``.
Entretanto as faixas das extremidades são descartadas, pois estas são utilizadas por operações dentro da rede:
```
    104.198.241.0   | Utilizado para representar o endereço da rede.
    104.198.241.127 | Utilizado como endereço de broadcast; Usado para enviar pacotes para todos os hosts dentro de uma rede.
```
Dessa forma podemos determinar que a faixa de endereços de IP que pode ser utilizadas é a ``104.198.241.1 - 104.198.241.126``. <br>
*Também podemos utilizar aplicativos como [IP calculator](https://www.calculator.net/ip-subnet-calculator.html) para encontrar informações relacionadas a rede.




[endereço exclusivo]: <https://www.kaspersky.com.br/resource-center/definitions/what-is-an-ip-address>
[duas partes]: <https://www.avast.com/pt-br/c-what-is-an-ip-address>
[privados]: <https://blog.sninformatica.com.br/2020/01/28/quam-nulla-porttitor-massa-id-neque-aliquam-vestibulum/>
[sub-redes]: <https://www.datarain.com.br/blog/tecnologia-e-inovacao/como-calcular-uma-mascara-de-sub-rede/>
[máscara correspondente]: <https://www.alura.com.br/artigos/como-calcular-mascaras-de-sub-rede>
[CIDR]: <https://www.datarain.com.br/blog/tecnologia-e-inovacao/o-que-e-cidr/>