# Simulador de Investimentos em Fundos Imobiliários (FIIs) no Excel

Ferramenta em Excel que transforma um aporte mensal em números concretos: quanto você terá de patrimônio, quanto vai receber de dividendos e como dividir o dinheiro entre os tipos de FIIs conforme o seu perfil.

> Projeto educacional, desenvolvido como Desafio de Projeto. As premissas são ilustrativas e **não são recomendação de investimento**. Rentabilidade passada não garante resultados futuros.

## O que a ferramenta responde

Você informa o aporte mensal, o patrimônio inicial, o Dividend Yield (DY), a valorização esperada das cotas, o prazo e o seu perfil. A planilha responde:

| Pergunta | Onde está a resposta |
|---|---|
| Quanto vou ter de patrimônio daqui a X anos? | Simulador, **Patrimônio acumulado (VF)** |
| Quanto vou ter investido do meu próprio bolso? | Simulador, **Total investido** |
| Quanto desse patrimônio é ganho (juros compostos)? | Simulador, **Ganho acumulado** |
| Quanto vou receber de dividendos por mês e por ano? | Simulador, **Dividendos mensais e anuais estimados** |
| Como dividir o aporte entre Tijolo, Papel, Fundo de Fundos e Híbrido? | Simulador, seção **Divisão do aporte por perfil** |
| Como o resultado muda se eu esperar mais (ou menos) tempo? | Aba **Cenarios**, de 2 a 30 anos, com gráfico |
| E se eu aportar mais, ou o DY for outro? | Altere as células amarelas e tudo recalcula |

### Exemplo (premissas padrão)

Aporte de R$ 1.000 por mês, patrimônio inicial de R$ 0, DY de 8% a.a., valorização de 3% a.a., perfil Moderado:

| Prazo | Total investido | Patrimônio | Dividendo mensal estimado |
|---|---|---|---|
| 5 anos | R$ 60.000 | R$ 78.430 | R$ 523 |
| 10 anos | R$ 120.000 | R$ 210.590 | R$ 1.404 |
| 20 anos | R$ 240.000 | R$ 808.545 | R$ 5.390 |
| 30 anos | R$ 360.000 | R$ 2.506.390 | R$ 16.709 |

No perfil Moderado, o aporte de R$ 1.000 é dividido em R$ 350 (Tijolo), R$ 350 (Papel), R$ 200 (Fundo de Fundos) e R$ 100 (Híbrido).

## Como usar

1. Abra `simulador_fiis.xlsx` e vá à aba **Simulador**.
2. Edite só as células amarelas (texto azul): aporte mensal, patrimônio inicial, DY anual, valorização anual, prazo (inteiro de 2 a 30, com validação) e perfil (lista suspensa: Conservador, Moderado ou Arrojado).
3. Leia os resultados nas seções 3 e 4. Para comparar prazos, use a aba **Cenarios**.
4. Para mudar a distribuição por perfil, edite os percentuais em azul na aba **Perfis** (cada linha deve somar 100%).

## Estrutura da planilha

- **Simulador**: premissas, taxas, resultados e divisão do aporte.
- **Perfis**: tabela de apoio com os percentuais por perfil, usada pelo PROCV.
- **Cenarios**: projeção ano a ano de 2 a 30 anos, com gráfico de patrimônio × total investido.

## Como cada cálculo funciona

1. **Retorno total anual** = `DY_Anual + Valorizacao_Anual` (dividendos reinvestidos mais valorização das cotas).
2. **Taxa mensal equivalente** = `(1 + Taxa_Anual)^(1/12) − 1`. Usa juros compostos, e não a divisão simples por 12, para que 12 meses rendam exatamente a taxa anual.
3. **Prazo em meses** = `Prazo_Anos × 12` (o `nper` da função VF).
4. **Total investido** = `Patrimonio_Inicial + Aporte_Mensal × Meses`.
5. **Patrimônio acumulado** = `VF(Taxa_Mensal; Meses; -Aporte_Mensal; -Patrimonio_Inicial)`. A função VF capitaliza o valor inicial e os aportes (feitos no fim de cada mês). Os sinais negativos fazem o resultado sair positivo.
6. **Ganho acumulado** = patrimônio − total investido.
7. **Dividendos mensais estimados** = `Patrimonio_Final × DY_Anual / 12` (renda passiva ao final do prazo). Os anuais são esse valor × 12.
8. **Divisão por perfil**: `PROCV(Perfil_Escolhido; Tabela_Perfis; coluna; FALSO)` busca na aba Perfis o percentual de cada segmento. O valor mensal é o percentual × `Aporte_Mensal`.
9. **Cenários**: repete os itens 3 a 7 para cada prazo de 2 a 30 anos, com as mesmas premissas.

### Intervalos nomeados

As fórmulas usam nomes em vez de endereços de célula, o que as deixa legíveis: `Aporte_Mensal`, `Patrimonio_Inicial`, `DY_Anual`, `Valorizacao_Anual`, `Prazo_Anos`, `Perfil_Escolhido`, `Taxa_Anual`, `Taxa_Mensal`, `Meses`, `Total_Investido`, `Patrimonio_Final`, `Dividendos_Mensais`, `Tabela_Perfis` e `Lista_Perfis`.

## Premissas e limitações

- DY de 8% a.a., valorização de 3% a.a. e os percentuais por perfil são valores didáticos escolhidos para o exemplo, sem fonte oficial. Troque pelos seus.
- Taxas constantes durante todo o prazo.
- Sem impostos, corretagem ou taxas de administração.
- Dividendos considerados totalmente reinvestidos durante o acúmulo; o valor de dividendos mostrado é uma estimativa ao final do prazo.

## Estrutura do repositório

```
simulador_fiis.xlsx
README.md
