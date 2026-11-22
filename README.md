# Comparação de Preços Ifood

Robô UiPath que compara o preço de uma mesma lista de compras em vários mercados do **iFood** e envia o
resultado por e-mail.

## Demonstração

[![Assista ao robô em execução](https://img.shields.io/badge/YouTube-Assista%20à%20demonstração-red?logo=youtube)](YOUTUBE_URL)

> 🎥 Vídeo do workflow em execução: **[YOUTUBE_URL](YOUTUBE_URL)**

## O que ele faz

1. Lê a lista de produtos e de mercados em uma planilha do Google.
2. Para cada mercado, monta o mesmo carrinho no iFood.
3. Lê os valores do checkout: subtotal, taxa de serviço, entrega, total, pedido mínimo e quanto falta para
   a entrega grátis.
4. Usa o Claude para formatar um e-mail HTML com os mercados lado a lado.
5. Envia o relatório pelo Gmail.

Se um mercado falhar, o robô segue para o próximo e mostra a falha no relatório. O passo a passo completo,
com diagrama, está em **[WORKFLOW.md](WORKFLOW.md)**.

## Tecnologias

- UiPath Studio 26.10 (VB.NET, Windows, .NET 10)
- UiPath UI Automation (Chrome)
- UiPath GSuite (Gmail e Google Drive)
- UiPath Integration Service (Claude API)
- UiPath Orchestrator (assets)

## Estrutura

```
Main.xaml                   Ponto de entrada: loop pelos mercados, relatório e tratamento de erros
Framework/
  InitSettings.xaml         Carrega Config.xlsx e os assets do Orchestrator
  ReadInputData.xaml        Baixa e valida a planilha de entrada
iFood/
  BotVerification.xaml      Pede ajuda ao usuário na verificação de robô
  SearchMarket.xaml         Busca e abre o mercado
  SearchProduct.xaml        Busca um produto
  AddProductNTimes.xaml     Adiciona N unidades ao carrinho
  ReadMarketInfo.xaml       Lê o selo de entrega grátis
  GetSubtotalMarket.xaml    Lê os valores do checkout
Report/
  BuildEmailHtml.xaml       Gera o e-mail HTML com o Claude
Data/
  Config.xlsx               Configurações e nomes dos assets
  EmailPrompt.txt           Prompt de sistema do e-mail
```

## Como configurar

1. **Planilha do Google** com duas abas:
   - `Produtos`: caixa de seleção na primeira coluna, mais as colunas `Item` e `Quantidade`.
   - `Mercados`: coluna `Mercado`, com o nome como aparece na busca do iFood.
2. **Assets no Orchestrator**, na pasta `ComparePricesMarkets`:
   - `SheetURL`: link de download da planilha.
   - `SendTo`: e-mail que recebe o relatório.
3. **Conexões no Integration Service**: Gmail / Google Drive e Anthropic (Claude).
4. Revise a aba `Settings` de `Data/Config.xlsx` (URL do iFood, nomes de abas e colunas, modelo do Claude,
   assunto do e-mail).

## Como executar

Abra o projeto no UiPath Studio e rode o `Main.xaml`, ou publique no Orchestrator e inicie o job.

> ⚠️ O processo precisa de um usuário por perto: se o iFood mostrar a verificação de robô, aparece uma caixa
> de mensagem pedindo para resolvê-la.

Pela linha de comando, com o [UiPath CLI](https://docs.uipath.com/coding-agents/standalone/latest/user-guide/install-and-set-up):

```powershell
uip rpa validate
uip rpa build
uip rpa run
```
