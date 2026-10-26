# Web-Scraping-Partner-List
Extrair a lista e as informações de parceiros de uma empresa

## Visão Geral do Projeto
#### Objetivo:
Automatizar a extração completa da lista de parceiros comerciais e institucionais cadastrados no portal oficial Energy Star.

#### Conceito & Escopo:
O scraping lida com interações dinâmicas de interface (troca de iframe, submissão de formulário e paginação controlada via JavaScript). A extração raspa todas as páginas disponíveis e consolida as informações em um DataFrame estruturado via Pandas.

## Fonte de Dados (Data Source)
#### Origem:
Portal Energy Star (All Partner List)
URL: [https://www.energystar.gov/partner-resources/partner-list/all-partner-list](https://www.energystar.gov/partner-resources/partner-list/all-partner-list)

#### Formato de Exibição:
Tabela em HTML incorporada dentro de um iframe.

#### Frequência / Acesso:
Extração sob demanda via automação de navegador (Selenium).

## Fluxo de Dados (ELT / Extração e Estruturação)
#### Navegação & Troca de Contexto:
O Selenium inicializa o Chrome Driver e acessa a página base.\
Aguarda o carregamento e realiza o switch_to.frame() para interagir com a tabela contida no iframe.

#### Disparo da Consulta & Cálculo de Paginação:
Executa o clique no botão de submissão (#submit-button) via execute_script.\
Captura o texto do elemento .table-info para extrair o total de itens registrados e calcula a quantidade total de páginas (considerando 20 itens por página).\
Leitura do Cabeçalho & Iteração Sequencial:\
Mapeia as colunas do cabeçalho (<th>) tratando limpezas de texto (remoção de quebras de linha e caracteres de ordenação).\
Percorre a tabela (#all-partner-list-table) extraindo os campos de cada linha (<tr> / <td>).\
Executa o clique programático no botão de "Próximo" (.next-button) com pausa estratégica (time.sleep(1)) para aguardar o renderizador da página.\

#### Consolidação:
Transforma a matriz de dados capturados em um pandas.DataFrame com as colunas correspondentes.

## Detalhes da Arquitetura & Desenvolvimento
#### Linguagem & Bibliotecas:
Python 3, selenium, webdriver_manager, pandas, math, time.

#### Tecnologia Principal de Scraping:
Selenium WebDriver (escolhido devido à renderização dinâmica via JavaScript/Salesforce e presença de iframe).

#### Estratégias de Resiliência:
* Uso de Explicit Waits (WebDriverWait + expected_conditions) para aguardar o carregamento de elementos cruciais (iframe, botões e tabela) e evitar erros de NoSuchElementException ou StaleElementReferenceException.
* Cliques executados via JavaScript (driver.execute_script("arguments[0].click();", ...)), garantindo a interação mesmo com elementos parcialmente cobertos ou com comportamentos específicos do DOM.

## Estrutura do Dataset Gerado
O DataFrame final contém as seguintes colunas (capturadas do cabeçalho dinâmico):
* Company Name (Nome da Organização/Parceiro)
* Partner Type (Tipo de Parceria)
* Cidade
* Estado
* País)
* Awards (prêmios recebidos).
