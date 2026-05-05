# Projeto BlazeDemo - Testes de Performance (JMeter)


## Pré-requisitos

- Java 8 ou superior instalado
- Apache JMeter 5.6.3 (incluído no projeto)
- Node.js e npm para executar scripts auxiliares


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
| `evidências/` | Screenshots e vídeos de execuções |

## Passo a Passo para Executar os testes

1. Certifique-se de que Java 8+ está instalado e configurado.
2. Execute `test:carga` para rodar os testes de performance (Carga) com JMeter.
3. Execute `test:pico` para rodar os testes de performance (Pico) com JMeter.
4. Os resultados serão salvos em `resultado.jtl` e relatórios em `logs/`.
5. Para limpar os arquivos gerados, execute `npm run limpar`.

## Rodando os testes no GitHub Actions

### Como executar manualmente

1. Acesse a aba **Actions** no repositório
2. Selecione o workflow **JMeter Tests**
3. Clique em **Run workflow**
4. Escolha a branch (ex: main)
5. Clique em **Run workflow**

### Visualizar relatório dos testes

Após a execução:
- O report é publicado automaticamente no GitHub Pages
- Acesse o link em: https://github.com/thiagosouza10/blazedemo-performance-jmeter/