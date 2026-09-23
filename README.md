# Rede Neural com Backpropagation — Projeto 

Projeto individual da disciplina de Matemática/Computação. A especificação pedia projetar e implementar uma rede neural a partir do exemplo de classe (`exemplo4.py` - aula de 21/08/2026), com liberdade para alterar neurônios, camadas, função de ativação e base de dados.

**Autora:** Carolina Cortez · cc2@cin.ufpe.br · **Entrega:** 26/09/2026

## O que foi feito

Implementei uma rede neural — forward e backpropagation escritos de forma escalar, uma operação por linha, seguindo a estrutura do `exemplo4.py` — e validei o resultado contra uma baseline equivalente em Keras. Foram dois experimentos, cada um em um notebook em formato de relatório (com diagrama da rede, dataset, fórmulas, curvas de loss e fronteira de decisão):

| Experimento | Dataset | Arquitetura | Ativações | Acurácia |
|---|---|---|---|---|
| [01 — XOR](experiments/ex01_xor/experimento_01.ipynb) | XOR (4 amostras) | `[2 → 2 → 1]` | sigmoide nas duas camadas | 100% |
| [02 — Two Moons](experiments/ex02_two_moons/experimento_02.ipynb) | `make_moons` (100 amostras) | `[2 → 4 → 1]` | tanh na oculta, sigmoide na saída | 100% |

## Modificações em relação ao `exemplo4.py`

- **Experimento 01 (XOR):** troca da base de dados — o XOR é o menor problema não linearmente separável e expõe um comportamento que o Two Moons não mostra: um *ponto de sela* longo na loss (~1500 épocas de platô antes de convergir).
- **Experimento 02 (Two Moons):** mesmo dataset do exemplo, com a camada oculta ampliada de 2 para 4 neurônios e a ativação trocada de sigmoide para tanh.

## Resultados principais

- As duas implementações manuais convergem para **100% de acurácia** e aprendem fronteiras de decisão coerentes (banda diagonal no XOR; região em Λ no Two Moons).
- A baseline Keras com a mesma arquitetura reproduz o resultado — **desde que a taxa de aprendizado seja compensada**: o treino manual *soma* os gradientes das N amostras (perda ½e²), enquanto o SGD do Keras usa a *média* (perda e²). Equivalência exata: `lr_keras = lr × N/2`.
- Sem essa compensação, o Keras fica preso no sela do XOR com `lr=0.1` — um lembrete de que "mesmo algoritmo" não significa "mesmo passo".

As anotações completas (convergência, inicialização, soma vs. média dos gradientes, bugs encontrados) estão na seção final de cada notebook.

## Estrutura do repositório

```
input/                       especificação do projeto e exemplo4.py de referência (aula de 21/08/2026)
experiments/ex01_xor/        notebook + fórmulas + figuras do experimento XOR
experiments/ex02_two_moons/  notebook + fórmulas + figuras do experimento Two Moons
```

## Como executar

Requer [uv](https://docs.astral.sh/uv/) e Python 3.14.5:

```bash
uv sync
uv run jupyter lab
```

Depois abra `experiments/ex01_xor/experimento_01.ipynb` ou `experiments/ex02_two_moons/experimento_02.ipynb` e rode as células em ordem.
