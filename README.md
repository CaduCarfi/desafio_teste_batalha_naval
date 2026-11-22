# desafio_teste_batalha_naval

Autor: **Carlos Eduardo Carfi Silva** (RA 24000663-2, UniCesumar — Engenharia de Software).

---

## Proposta do desafio

Motor de Batalha Naval para dois jogadores, cada um com seu próprio tabuleiro **10×10** (índices de 0 a 9). Cada jogador posiciona exatamente a frota pedida no enunciado:

| Navio | Tamanho |
|---|---|
| Porta-aviões | 5 casas |
| Encouraçado | 4 casas |
| Submarino | 3 casas |
| Destróier (ou Hidroavião) | 2 casas |

Optei pela nomenclatura "destróier" entre as duas alternativas do enunciado ("Destroier ou Hidroavião") — é só um rótulo, o comportamento é o mesmo.

## Regras implementadas

- Navios são posicionados na horizontal (`H`) ou vertical (`V`), sempre sem sair do tabuleiro e sem sobrepor outro navio já colocado.
- Um posicionamento inválido não ocupa nenhuma casa — ou entra tudo, ou nada.
- Depois do primeiro tiro disparado por qualquer jogador, não é mais possível reposicionar navios.
- Um tiro pode resultar em `água`, `acerto` (e informa qual navio foi atingido) ou `afundou` (quando todas as casas daquele navio já foram atingidas).
- Tiro repetido na mesma casa, ou em coordenada fora do tabuleiro, é rejeitado.
- Para a partida (classe `BatalhaNaval`) funcionar, as duas frotas precisam estar completas antes do primeiro tiro; os jogadores se alternam a cada tiro válido (inclusive quando acertam), e destruir as 14 casas da frota adversária declara o vencedor e trava novos tiros.
- Não impus distância mínima entre navios adjacentes, porque isso não está no enunciado.

## Estrutura

```
src/batalha-naval.js        -> classes Tabuleiro e BatalhaNaval
test/batalha-naval.test.js  -> 11 testes cobrindo posicionamento, tiros e partida completa
evidencias/testes.log       -> log da última execução
.github/workflows/testes.yml -> CI
```

## Executando

```bash
npm test
npm run coverage
```

(Requer só Node.js 22+; sem dependência externa.)

## Exemplo de uso

```javascript
import { Tabuleiro } from './src/batalha-naval.js';

const frota = new Tabuleiro();
frota.posicionar('porta-avioes', 0, 0, 'H');
frota.posicionar('encouracado', 1, 0, 'H');
frota.posicionar('submarino', 2, 0, 'H');
frota.posicionar('destroier', 3, 0, 'H');

console.log(frota.atirar(3, 0)); // { resultado: 'acertou', navio: 'destroier' }
```

## Casos de teste

| Caso | Cobertura |
|---|---|
| CT01 | A frota exige exatamente os 4 navios, nos tamanhos corretos |
| CT02 | Posicionamento horizontal colado na última coluna é aceito |
| CT03 | Posicionamento vertical colado na última linha é aceito |
| CT04 | Navio que sairia do tabuleiro não ocupa nenhuma casa |
| CT05 | Sobreposição, navio repetido, tipo/direção inválidos são rejeitados |
| CT06 | Um tiro distingue corretamente água, acerto e afundamento |
| CT07 | Tiro repetido ou em coordenada inválida é rejeitado |
| CT08 | Frota precisa estar completa antes do combate; sem reposicionar depois do 1º tiro |
| CT09 | Afundar as 14 casas da frota marca ela como destruída |
| CT10 | Partida exige as duas frotas prontas, jogador válido, e alterna a vez corretamente |
| CT11 | Ao fim da partida há um vencedor e novos tiros são bloqueados |

## Resultado da última execução

Node.js 22.19.0 — **11 de 11 testes passando**, nenhuma falha. Saída integral em [`evidencias/testes.log`](evidencias/testes.log). Cobertura de `src/batalha-naval.js`: 100% (linhas, ramificações e funções).

## Colaborador(es) / entrega

- **Carlos Eduardo Carfi Silva** — RA 24000663-2. Desafio feito individualmente, repositório próprio (`desafio_teste_batalha_naval`), como exige o padrão de nomenclatura da disciplina.
