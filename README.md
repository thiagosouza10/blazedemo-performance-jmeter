# Projeto BlazeDemo - Testes de Performance (JMeter)


## Pré-requisitos

- Java 8 ou superior instalado
- Apache JMeter 5.6.3 (Já deixei incluído no projeto)
- Node.js e npm para executar scripts auxiliares

## Documentação

**Cenário:**
- Compra de passagem aérea - Passagem comprada com sucesso no site https://www.blazedemo.com/.

**Critério de Aceitação:**
- 250 requisições por segundo com um tempo de resposta 90th percentil inferior a 2 segundos.


## Arquitetura

O projeto utiliza Apache JMeter para testes de performance no site BlazeDemo. A estrutura do teste inclui:

- **Test Plan**: Plano principal com variáveis definidas (BASE_URL, SCHEME)
- **HTTP Cookie Manager**: Gerenciamento de cookies
- **HTTP Cache Manager**: Gerenciamento de cache
- **Thread Groups**: Grupos de threads para simular carga de usuários
- **HTTP Samplers**: Requisições HTTP para páginas do BlazeDemo (home, reserva, compra, etc.)
- **Listeners**: Para coletar resultados e gerar relatórios

## Pasta docs

| Pasta | Descrição |
|-------|-----------|
| `cenarios-de-testes/` | Cenários de teste documentados |
| `evidências/` | Relatórios HTML dos testes executados e também um relatório de feedback dos testes relatorio.md|

## Passo a Passo para Executar os testes

1. Certifique-se de que Java 8+ está instalado e configurado.
2. Execute `npm run test:load` para rodar os testes de performance (Carga) com JMeter.
3. Execute `npm run test:spike` para rodar os testes de performance (Pico) com JMeter.
4. Os resultados serão salvos em `resultado.jtl` e relatórios em `logs/`.
5. Para limpar os arquivos gerados, execute `npm run limpar`.

## Rodando os testes no GitHub Actions

### Como executar manualmente

1. Acesse a aba **Actions** no repositório
2. Selecione o workflow **JMeter Tests**
3. Clique em **Run workflow**
4. Escolha a branch (ex: main)
5. Escolha o tipo de teste `test:load` ou `test:spike`
6. Clique em **Run workflow**

### Visualizar relatório dos testes

Após a execução:
- O report é publicado automaticamente no GitHub Pages
- Acesse o link em: https://thiagosouza10.github.io//blazedemo-performance-jmeter/
