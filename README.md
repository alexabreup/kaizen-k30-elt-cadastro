# Projeto Kaizen K30 - Reestruturacao Logistica e Governanca Cadastral (Eletromidia S.A.)

Este repositorio contem a modelagem de processos, os dados de auditoria e a especificacao de arquitetura de TI do Projeto Kaizen K30, desenvolvido para a Eletromidia S.A. 

O objetivo principal deste projeto e estruturar e padronizar a governanca cadastral do MUBI Nacional no SAP sob as diretrizes globais das normas ISO 8000 (Qualidade de Dados Mestre) e ISO 55000 (Gestao de Ativos), integrando Suprimentos, Engenharia, Manutencao e a area Fiscal em um fluxo operacional blindado contra erros e duplicidades.

Este repositorio foi projetado para funcionar de forma independente e autoexplicativa. Ele reune toda a inteligencia de negocios, diagramas arquiteturais e relatorios analiticos necessarios para que qualquer programador ou arquiteto de solucoes de TI possa dar continuidade a implementacao e colocar os sistemas em producao.

---

## 1. O Desafio de Negocio e Diagnostico da Base

A Eletromidia S.A. opera milhares de ativos de midia Out-of-Home (OOH) em nivel nacional. A eficiencia logistica e o compliance tributario desses ativos dependem diretamente de uma base de dados de materiais integra no SAP ERP. 

Uma auditoria programatica profunda executada em Junho de 2026 sobre a base de dados mestre revelou os seguintes indicadores estruturais:
* Total de Itens Auditados: 14.799 registros.
* Cadastros Ativos: 10.276 itens (69,44% do total).
* Duplicidades Ativas: 1.561 codigos identificados com descricoes redundantes gerando estoques inflados.
* Conformidade Taxonomica: Menos de 10% dos cadastros seguiam o Padrao de Descricao de Materiais (PDM).

A ausencia de governanca rigida resultava em:
* Inconsistencias de Cadastro: Cadastros livres e manuais duplicavam codigos e causavam redundancia nas compras de suprimentos.
* Riscos Fiscais: Nomenclaturas Comuns do Mercosul (NCM) incorretas ou ausentes sujeitavam a companhia a autuacoes e retencao de notas de transporte.
* Perda de Eficiencia em Campo: Falta de padronizacao impedia o MRP (planejamento automatico de reposicao) e induzia tecnicos de campo a retirar componentes errados no almoxarifado.

---

## 2. Estrutura do Repositorio

O repositorio esta estruturado para manter toda a inteligencia, documentos e a demonstracao interativa de forma limpa e organizada:

```text
kaizen-k30/
├── .github/                             # Pipelines de automacao (GitHub Actions)
│   └── workflows/
│       └── deploy.yml                   # Deploy automatico da pasta html para o GitHub Pages
├── .git/                                # Versionamento do repositorio
├── .gitignore                           # Exclusoes de arquivos temporarios do Office e do sistema
├── LICENSE                              # Termos de propriedade e confidencialidade corporativa
├── README.md                            # Este manual de arquitetura e especificacao tecnica
│
├── html/                                # Pasta da demonstracao interativa do portal
│   └── index.html                       # Versao monopagina do Guia Executivo e Simulador de Bot
│
├── Abertura Kaizen BOM.docx             # Ata de abertura e escopo da frente de engenharia de materiais
├── apresentacao_executiva_k30_bot.md    # Roteiro e proposta de negocio para o Chatbot K30-BOT
├── Cadastro_Itens.xlsx                  # Dados brutos da auditoria programatica (14.799 itens)
├── Governança ISO 8000 Eletromidia.pdf  # Estudo detalhado sobre conformidade e qualidade de dados mestre
├── guia-executivo-k30.md                # Guia executivo consolidado do projeto Kaizen K30
├── Kaizen_Apresentacao_BOM.pptx.pdf     # Slides de arquitetura de Bill of Materials (BOM) e kits
└── kaizen-landing-page.md               # Modelo estrutural e menu do Portal de Governanca OOH
```

---

## 3. Especificacao Tecnica da Arquitetura do K30-BOT

O K30-BOT e um assistente conversacional inteligente que atua como um portao de seguranca (Poka-Yoke ou sistema a prova de erros) para impedir novos cadastros fora da norma no SAP. O fluxo de interacao, dados e infraestrutura e detalhado abaixo.

### 3.1 Poka-Yoke e Taxonomia ISO 8000
Todo novo cadastro de material deve obrigatoriamente seguir a taxonomia Noun-Modifier (Substantivo-Modificador) da ISO 8000:
```text
[SUBSTANTIVO], [MODIFICADOR 1], [MODIFICADOR 2], ..., [FABRICANTE/MODELO], [ESPECIFICACOES]
```
O bot elimina a insercao de texto livre manual pelo usuario, guiando-o atraves de perguntas estruturadas (Modificador 1, Modificador 2, etc.) para montar a nomenclatura final em caixa alta padronizada.

### 3.2 Infraestrutura e Fluxo de Dados na GCP (Google Cloud Platform)

A arquitetura de TI projetada e serverless, escalavel e altamente integrada:

1. Interface de Chat (Slack/Teams):
   * O usuario final inicia o processo atraves do Slack de forma familiar e sem necessidade de treinamento no SAP.
   * Framework de Conexao: Slack Bolt for JavaScript/TypeScript ou Python Bolt.

2. Cérebro da Aplicacao (Cloud Run):
   * Hospeda o backend serverless desenvolvido em NodeJS ou Python.
   * Recebe as requisicoes do Slack, executa as validacoes de negocio, coordena os chamados de IA e faz a interface com o banco de dados e com o SAP ERP.

3. Inteligencia Artificial (Vertex AI - Gemini Model):
   * Executa a analise de similaridade semantica das solicitacoes do usuario em tempo real.
   * Sugere automaticamente a descricao padronizada e o respectivo NCM fiscal com base no banco de dados mestre alimentado com a taxonomia PDM.
   * Traduz a linguagem natural informal de compradores para a nomenclatura estruturada.

4. Camada de Dados (Cloud Firestore & BigQuery):
   * Cloud Firestore: Banco de dados NoSQL de baixa latencia para persistir logs de auditoria, historico de mensagens e cadastros de materiais pendentes de homologacao.
   * BigQuery: Data Warehouse para analise de conformidade historica e geracao de KPIs de governanca.

5. Barreira de Duplicidade (Integracao SAP MDM):
   * Antes de persistir qualquer item, o Cloud Run executa uma busca na tabela mestre do SAP. Caso ja exista um PDM equivalente cadastrado, o bot bloqueia a criacao do novo codigo e sugere o reuso do codigo existente.

---

## 4. Estrutura de Kitting, MRP e Rastreabilidade

O projeto arquitetonico estende-se da governanca logica para a integracao fisica em campo:

### 4.1 Estrutura BOM Padronizada (Kitting)
* Diretriz: Agrupamento previo de componentes, cabos e acessorios associados a Bill of Materials (BOM) de cada modelo do MUBI Nacional.
* Funcionamento: O tecnico de manutencao recebe o kit consolidado e validado antes de se deslocar para campo, suprimindo paradas operacionais por pecas esquecidas e compras emergenciais de ultima hora fora do portal.

### 4.2 Reposicao Inteligente (MRP via SAP PM/MM)
* Parametrizacao de niveis criticos de estoque no SAP associados diretamente a integridade dos cadastros mestre. O sistema passa a disparar solicitacoes de compras de forma autonoma quando as pecas do MUBI atingem o ponto de ressuprimento.

### 4.3 Linhagem de Dados (Data Lineage) e Rastreabilidade Fisica
* Linhagem de Dados: Mapeamento logico rigoroso ("De-Para") documentando a conversao de cadastros historicos legados para a nova taxonomia ISO 8000. Isso impede a perda de historico contabil e de manutencao dos ativos.
* Rastreabilidade Fisica: Integracao de leitores de campo (QR Code/DataMatrix) nos paineis OOH que atualizam o status das pecas em tempo real no modulo EAM/SAP PM da Eletromidia.

---

## 5. Manual de Implementacao para o Desenvolvedor

Qualquer programador encarregado de tirar o K30-BOT e o sistema de governanca do papel devera seguir as seguintes instrucoes tecnicas de desenvolvimento:

1. Configurar o Workspace do Slack:
   * Criar uma aplicacao em api.slack.com/apps.
   * Assinar os escopos de bot: chat:write, commands, app_mentions:read e im:history.
   * Configurar URLs de Interatividade para capturar acoes dos botoes de escolha do bot.

2. Desenvolver o Backend (Cloud Run):
   * Desenvolver em NodeJS ou Python.
   * Integrar a SDK oficial Google Gen AI para requisicoes ao Vertex AI Gemini Pro/Flash.
   * Implementar o algoritmo de Poka-Yoke que monta a string PDM a partir das respostas estruturadas do usuario.

3. Implementar a Busca por Similaridade (IA/DB):
   * Converter os cadastros existentes do SAP (fornecidos em Cadastro_Itens.xlsx) em vetores de busca (Embeddings).
   * Utilizar busca vetorial ou algoritmos de similaridade (como Levenshtein/Trigram) para comparar o texto digitado pelo usuario com a base mestre, emitindo alertas de duplicidade quando a similaridade for superior a 85%.

4. Estabelecer as Conexoes SAP BTP:
   * Configurar chamadas OData ou RFC do modulo SAP PM/MM para consultar estoques e disparar transacoes de cadastro de materiais apos homologacao do comite de governanca.

---

## 6. Publicacao Automatica no GitHub Pages

Este repositorio esta configurado para publicar automaticamente a demonstracao interativa contida na pasta `html/` (o arquivo `index.html`) como um website hospedado no **GitHub Pages**.

Toda vez que uma alteracao e enviada para a branch `main` no GitHub, um pipeline de automacao (GitHub Action) e disparado para atualizar o site sem que voce precise compilar nada manualmente.

### Como Ativar o Website no seu Repositorio do GitHub:
1. Acesse o seu repositorio no site do GitHub.
2. No menu superior, clique em **Settings** (Configuracoes).
3. Na barra lateral esquerda, clique em **Pages** (sob a secao *Code and automation*).
4. Em **Build and deployment**:
   * Em **Source** (Origem), mude de *Deploy from a branch* para **GitHub Actions**.
5. Pronto! O pipeline de automacao em `.github/workflows/deploy.yml` cuidara do resto. 
6. Em alguns instantes, o link do seu site publicado estara visivel no topo desta mesma tela de configuracao (com o formato `https://seu-usuario.github.io/kaizen-k30/`).

---

## 7. Licenca

Este projeto e de propriedade estrita e confidencial da Eletromidia S.A. Todo o conteudo tecnico, dados analiticos e projetos de arquitetura sao confidenciais e protegidos por leis de propriedade intelectual. A distribuicao, copia ou compartilhamento nao autorizados sao expressamente proibidos.

---

*Especificacoes de arquitetura consolidadas pela Equipe Kaizen K30 - Clever Solucoes Inteligentes - Julho de 2026.*
