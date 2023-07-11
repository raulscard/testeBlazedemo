## Script para demo Blazedemo
Script para teste de performance no cenário de compra de passagem aérea em "blazedemo.com"


## Pré requisitos
- Jmeter (utilizada a versão 5.6.1)
	- Plugins utilizados: Plugin Manager, Custom Thread Groups e Throughput Shaping Timer
- Python (Versão utilizada 3.11.4)
- Taurus - [Documentação](https://gettaurus.org/install/Installation/)

## Instruções
Os scripts serão executados utilizando a ferramenta Taurus, para executar se deve rodar o comando:

```bash
bzt <Arquivo de configuração do Taurus>
```

Exemplos
```bash
bzt configTesteCarga.yml
bzt configTestePico.yml
```

## Relatório das execuções

### Teste de Pico

```bash
+---------------+---------------+
| Percentile, % | Resp. Time, s |
+---------------+---------------+
|          90.0 |         1.163 |
|         100.0 |          4.14 |
+---------------+---------------+
```
```bash
+-------+--------+--------+--------+---------------------------------------------------------------------+
| label | status |   succ | avg_rt | error                                                               |
+-------+--------+--------+--------+---------------------------------------------------------------------+
| Agi   |  FAIL  |  0.00% |  0.744 | Number of samples in transaction : 1, number of failing samples : 1 |
| Home  |  FAIL  | 99.85% |  0.938 | Too Many Requests                                                   |
+-------+--------+--------+--------+---------------------------------------------------------------------+
```

Como podemos observar nas tabelas acima, existe uma diferença considerável entre o tempo médio de resposta de todo o teste e do P90, com o P90 chegando a atender os requisitos não funcionais, porém, com a alta taxa de erros esses tempos de resposta não podem ser considerados para análise de resultado com a execução chegando praticamente aos 100% de erro para todo o teste. Como os erros foram "Too Many Requests", isso indica que a aplicação possui algum rate limit configurado.

### Teste de Carga

```bash
+---------------+---------------+
| Percentile, % | Resp. Time, s |
+---------------+---------------+
|          90.0 |         8.408 |
|         100.0 |        12.288 |
+---------------+---------------+
```
```bash
+-------+--------+--------+--------+---------------------------------------------------------------------+
| label | status |   succ | avg_rt | error                                                               |
+-------+--------+--------+--------+---------------------------------------------------------------------+
| Agi   |  FAIL  |  0.00% |  0.765 | Number of samples in transaction : 1, number of failing samples : 1 |
| Home  |  FAIL  | 79.51% |  3.399 | Too Many Requests                                                   |
+-------+--------+--------+--------+---------------------------------------------------------------------+
```

No teste de carga podemos perceber tempo de resposta maior no P90, chegando mais próximo a média de todo o teste. Com a alta taxa de erros novamente não é possível mensurar a experiência do usuário, apesar do tempo de resposta consideravelmente acima do requisito, isso pode não refletir a experiência real de um usuário ao não ser bloqueado pelo rate limit.


## Considerações

Como foi apenas um teste em site demo, podemos entender que o rate limit cumpriu a sua função impedindo esse grande volume de requisições. Em um planejamento de teste de performance seria levantada a necessidade de desativação do rate limit durante a janela de execução ou o estudo de estratégias de bypass.
