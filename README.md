# Atividade de Recuperação: Astronautas — Recursos Extras

Este projeto parte do sistema da Unidade 1 e adiciona os recursos descritos na
atividade de recuperação

## Camada 1 — Funções dos astronautas 

- `Astronauta` passou de `struct` para `class` base, com o método virtual
  `funcao()` (retorna `"Astronauta"` por padrão) e destrutor virtual.
- Foram criadas quatro classes derivadas, cada uma sobrescrevendo `funcao()`:
  `Comandante`, `Engenheiro`, `Cientista` e `Turista`.
- Os astronautas passaram a ser guardados em `vector<Astronauta*>`, e uma
  função fábrica (`criarAstronauta`) instancia o tipo correto a partir da
  palavra lida no comando (`COMANDANTE`, `ENGENHEIRO`, `CIENTISTA` ou
  `TURISTA`). Se a função informada não for reconhecida, o cadastro é
  recusado com mensagem de erro.
- O comando `CADASTRAR_ASTRONAUTA` mudou para receber a função antes do CPF:
  `CADASTRAR_ASTRONAUTA funcao cpf idade nome completo`.
- `LANCAR_VOO` ganhou uma nova validação: o voo só pode ser lançado se tiver
  pelo menos um Comandante e pelo menos um Engenheiro entre seus passageiros.
- `LISTAR_VOOS` e `LISTAR_MORTOS` agora mostram também a função de cada
  astronauta (chamando o método polimórfico `funcao()`).

## Camada 2 — Naves

- Nova classe `Nave`, com código, nome e capacidade.
- Novo comando `CADASTRAR_NAVE codigo capacidade nome completo`, que recusa
  código duplicado e capacidade menor ou igual a zero.
- Novo comando `ASSOCIAR_NAVE codigo_voo codigo_nave`, que associa uma nave a
  um voo ainda planejado.
- `ADICIONAR_ASTRONAUTA` passou a respeitar a capacidade da nave associada ao
  voo (se houver uma): não é possível ultrapassar o limite de tripulantes.
- `LANCAR_VOO` ganhou mais uma validação: o voo só pode ser lançado se tiver
  uma nave associada.
- `LISTAR_VOOS` agora também mostra a nave associada a cada voo (ou
  "(nenhuma)", quando não há).


## Como compilar e executar

```
g++ -std=c++17 -Wall -o agencia agencia.cpp
./agencia < eventos.txt
```
