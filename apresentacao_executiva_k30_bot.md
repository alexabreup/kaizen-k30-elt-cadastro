# Apresentação Executiva — Projeto K30-BOT
## Chatbot Inteligente de Cadastro de Itens | Eletromidia

**Destinatários:** Departamentos Fiscal e Compras  
**Objetivo:** Apresentar o novo sistema de cadastro assistido por IA que elimina erros fiscais, duplicidades de estoque e compras emergenciais.

---

## 1. O Problema que Resolvemos

### Situação Atual (Caos)

| Problema | Impacto no Fiscal | Impacto em Compras |
|----------|-------------------|-------------------|
| Cadastros livres sem padrão (ex: "Parafuso", "Par. Sext.") | NCMs ausentes ou errados → autuações e retenção de notas | Códigos duplicados → estoque inflado e compras redundantes |
| Texto livre no sistema | Impossível validar tributação na origem | Busca ineficiente → técnico leva peça errada para campo |
| Compras fora do portal oficial | Notas sem classificação fiscal correta | Custos emergenciais, fretes extras, juros e protestos |
| Falta de rastreabilidade | Impossível auditar ciclo de vida do ativo | Sem previsibilidade de reposição (MRP) |

> **Resultado:** Dinheiro perdido em impostos a mais, multas da Receita Federal e operações paradas por falta de peças.

---

## 2. A Solução: O Chatbot K30-BOT

### Conceito Simples

Um **assistente virtual dentro do Slack** (ferramenta que o time já usa) que **guiá o usuário passo a passo** no cadastro de qualquer item — como um colega experiente sentado ao lado, que nunca esquece as regras.

### Como Funciona na Prática

**Cenário:** O técnico de campo precisa cadastrar um novo parafuso no sistema.

**ANTES (Processo Manual):**
1. Técnico abre o SAP
2. Digita "Par. sext. M8 inox" (texto livre)
3. Fiscal descobre depois que o NCM está errado → retrabalho
4. Compras descobre que já existiam 3 códigos iguais → estoque inflado

**DEPOIS (Com o K30-BOT):**
1. Técnico manda mensagem no Slack: `@k30bot cadastrar parafuso`
2. O bot pergunta: *"Qual o tipo?"* → Sextavado
3. O bot pergunta: *"Qual a medida?"* → M8x40
4. O bot pergunta: *"Qual o material?"* → Inox
5. **O bot preenche automaticamente:**
   - Descrição padronizada: `PARAFUSO, SEXTAVADO, M8X40, INOX`
   - NCM fiscal correto vinculado na origem
   - Validação anti-duplicidade em tempo real
6. O bot confirma: *"Item cadastrado. Código #12345. NCM 7318.15.00. Risco fiscal: ZERO."*

---

## 3. Os Três Pilares de Segurança

### Pilar 1: Poka-Yoke (Anti-Erro)

**Tradução:** "A prova de erro" — o sistema não deixa o usuário errar.

| Proteção | Como Funciona | Benefício para Fiscal/Compras |
|----------|---------------|-------------------------------|
| Campos obrigatórios com máscara | O bot só aceita respostas no formato correto | Nunca mais haverá cadastro incompleto |
| Validação em tempo real | Ao digitar "parafuso", o bot já busca se existe igual | Duplicidade eliminada no ato do cadastro |
| NCM travado na origem | O bot vincula a classificação fiscal correta automaticamente | Notas fiscais de transporte sem retenção |
| Fórmula PDM (Noun-Modifier) | Toda descrição segue o padrão internacional ISO 8000 | Terminologia única entre Engenharia, Compras e Fiscal |

### Pilar 2: Inteligência Artificial (Gemini)

**Tradução:** O bot "aprende" e "pensa" para acelerar o trabalho humano.

| Funcionalidade de IA | Exemplo Prático | Resultado |
|---------------------|-----------------|-----------|
| **Autocompletar descrições** | Usuário digita "monitor 21 polegadas" → IA sugere "MONITOR, LCD, 21.5 POLEGADAS, POSITIVO MASTER" | Cadastro 5x mais rápido |
| **Sugestão de NCM** | IA identifica que é "monitor LCD" → já propõe NCM 8528.59.20 | Fiscal não precisa consultar tabela manualmente |
| **Detecção de similaridade** | IA compara com 10 mil itens existentes e avisa: "Já existe código #8842 com 95% de similaridade" | Compras evita comprar o mesmo item duas vezes |
| **Classificação automática** | IA reconhece "modem celular" → classifica como telecomunicação → NCM 8517.62.55 | Compliance fiscal desde o primeiro minuto |

### Pilar 3: Governança ISO 8000 + ISO 55000

**Tradução:** Padrão internacional que garante que todo dado seja confiável, rastreável e útil para o negócio.

| Princípio ISO 8000 | O que Significa na Prática | Ligação com ISO 55000 (Gestão de Ativos) |
|-------------------|---------------------------|------------------------------------------|
| **Sintático** | Formato padronizado (ex: sempre maiúsculas, vírgulas separando) | A "árvore de ativos" no SAP reflete a realidade física dos painéis OOH |
| **Semântico** | Significado claro e único ("parafuso sextavado" sempre significa a mesma coisa) | Manutenção preventiva funciona porque todos falam a mesma língua |
| **Pragmático** | Dado útil para o processo de negócio (Supply Chain, Fiscal, Engenharia) | O MRP (reposição automática) só funciona com dados íntegros |

---

## 4. Arquitetura em Linguagem de Negócio

### Onde Fica Cada Coisa (Google Cloud)

```
┌─────────────────────────────────────────────────────────────┐
│                    GOOGLE CLOUD PLATFORM                     │
│  (Infraestrutura segura, escalável e certificada)           │
├─────────────────────────────────────────────────────────────┤
│  SLACK (Interface)                                          │
│  └── O usuário conversa com o bot como fala com colega      │
│                                                             │
│  CLOUD RUN (Motor do Bot)                                   │
│  └── Processa a conversa, aplica regras de negócio          │
│                                                             │
│  VERTEX AI / GEMINI (Cérebro da IA)                         │
│  └── Entende o que o usuário quer e sugere preenchimentos   │
│                                                             │
│  FIRESTORE (Banco de Dados)                                 │
│  └── Guarda: itens cadastrados, histórico de conversas,     │
│      regras fiscais, taxonomia ISO 8000                     │
│                                                             │
│  CLOUD TASKS (Fila de Processamento)                        │
│  └── Garante que nada se perde se o sistema ficar lento     │
│                                                             │
│  SECRET MANAGER (Cofre de Senhas)                           │
│  └── Tokens e chaves de acesso protegidos                   │
└─────────────────────────────────────────────────────────────┘
```

### Por Que o Google Cloud?

| Critério | Benefício para Eletromidia |
|----------|---------------------------|
| Segurança | Certificações internacionais (SOC 2, ISO 27001) — dados fiscais protegidos |
| Escalabilidade | Se cadastrarem 10 ou 10 mil itens, o sistema não trava |
| Custo | Paga-se apenas pelo que usa (sem servidor ocioso) |
| Disponibilidade | 99,95% de uptime — sistema sempre no ar para Compras e Fiscal |

---

## 5. Fluxo de Cadastro Completo (Visão Fiscal + Compras)

### Passo a Passo do Novo Processo

```
┌─────────────┐     ┌─────────────┐     ┌─────────────┐     ┌─────────────┐
│   ENTRADA   │────▶│  VALIDAÇÃO  │────▶│   IA/GEMINI │────▶│  CONCLUSÃO  │
│   (Slack)   │     │  (Poka-Yoke)│     │  (Enriquece)│     │  (SAP/ERP)  │
└─────────────┘     └─────────────┘     └─────────────┘     └─────────────┘
      │                   │                   │                   │
      ▼                   ▼                   ▼                   ▼
  Usuário digita      Bot verifica:      IA sugere:          Item criado
  "cadastrar item"    - Campos           - Descrição         no SAP com:
                      obrigatórios?        padronizada         - Código único
                      - Duplicidade?     - NCM correto         - NCM validado
                      - Formato ISO?     - Classificação       - Taxonomia ISO 8000
                                          técnica             - Rastreabilidade
```

### Exemplo Real: Cadastro de Monitor LCD

| Etapa | O que o Usuário Vê | O que Acontece nos Bastidores |
|-------|-------------------|------------------------------|
| 1 | `@k30bot novo item` | Bot inicia conversa guiada |
| 2 | "Monitor LCD 21.5" | IA identifica: categoria = Interface Visual |
| 3 | Bot pergunta: "Marca?" | Usuário responde: "Positivo" |
| 4 | Bot pergunta: "Modelo?" | Usuário responde: "Master" |
| 5 | Bot confirma: | Sistema valida: NCM 8528.59.20 (Monitores LCD) |
| | `MONITOR, LCD, 21.5 POLEGADAS, POSITIVO, MASTER` | Verifica duplicidade: NENHUMA encontrada |
| | `NCM: 8528.59.20` | Grava no banco com selo ISO 8000 |
| | `Status: Apto para compra e nota fiscal` | Disponível para MRP (reposição automática) |

---

## 6. Benefícios Diretos para Cada Área

### Para o Departamento Fiscal

| Antes | Depois (com K30-BOT) |
|-------|---------------------|
| NCMs ausentes ou errados em 30% dos itens | NCM vinculado na origem — 100% dos itens classificados corretamente |
| Retenção de notas fiscais de transporte | Notas liberadas automaticamente — zero retenção por erro de classificação |
| Autuações e multas da Receita Federal | Blindagem fiscal proativa — risco tributário mitigado |
| Retrabalho de classificação após o cadastro | Classificação fiscal é feita UMA vez, no ato do cadastro |
| Impossibilidade de auditar base de materiais | Base 100% rastreável e auditável (Data Lineage) |

### Para o Departamento de Compras

| Antes | Depois (com K30-BOT) |
|-------|---------------------|
| 3, 5 ou 10 códigos para o mesmo parafuso | Um código mestre único por item físico |
| Compras emergenciais fora do portal | Compras 100% via portal — previsibilidade total |
| Técnico vai a campo sem peça correta | Kitting padronizado — técnico recebe kit validado no almoxarifado |
| Sem visão de ponto de ressuprimento | MRP automático — SAP gera pedido quando atinge estoque mínimo |
| Desperdício com materiais incorretos | Especificação técnica padronizada — compra certa na primeira vez |
| Fretes extras e custos imprevistos | Custo total de propriedade (TCO) reduzido |

---

## 7. Taxonomia Técnica: O Dicionário de Dados

### A Fórmula Padrão (ISO 8000 — Noun-Modifier)

Toda descrição de item segue a mesma estrutura, eliminando ambiguidade:

```
[SUBSTANTIVO], [MODIFICADOR 1], [MODIFICADOR 2], ..., [FABRICANTE], [ESPECIFICAÇÕES]
```

### Exemplos de Conversão

| Situação Atual (Caos) | Padrão ISO 8000 (Controle) | Categoria Técnica |
|----------------------|---------------------------|-------------------|
| "Par. Sext. M8 Inox" | `PARAFUSO, SEXTAVADO, M8X40, INOX` | Estrutura Mecânica |
| "Monitor 21" | `MONITOR, LCD, 21.5 POLEGADAS, POSITIVO, MASTER` | Interface Visual |
| "Modem vivo" | `MODEM, CELULAR, ELSYS, VIVO, 4G + ANTENAS` | Painel Elétrico/Automação |
| "Cabo HDMI" | `CABO, HDMI, 2 METROS, PREMIUM` | Conectividade |
| "Fonte 12V" | `FONTE, ALIMENTACAO, 12V, 5A, CHAVEADA` | Painel Elétrico/Automação |

### Categorias da Estrutura BOM (Bill of Materials)

| Categoria | Exemplos de Itens | Aplicação OOH |
|-----------|-------------------|---------------|
| **1. Estrutura Mecânica** | Parafusos, buchas, eletrodutos, abraçadeiras, conduletes | Fixação e infraestrutura física do painel |
| **2. Interface Visual** | Monitores LCD, calhas de acabamento, vidros, suportes | Exibição de mídia no ponto |
| **3. Painel Elétrico/Automação** | Computadores NUC, modems, switches, fontes, cabos | Conectividade e processamento |
| **4. Ferramental Padronizado** | Chaves combinadas, alicates, testadores, multímetros | Manutenção de campo |

---

## 8. NCM e Classificação Fiscal em Detalhe

### O que é o NCM e Por Que é Crítico

O **NCM (Nomenclatura Comum do Mercosul)** é o código de 8 dígitos que define como cada item é tributado no Brasil (IPI, ICMS, PIS/COFINS, II). Um NCM errado não é só um "detalhe técnico" — ele gera efeito cascata:

| Consequência de NCM Errado | Impacto Real |
|----------------------------|---------------|
| Alíquota de IPI/ICMS incorreta | Recolhimento a maior (dinheiro perdido) ou a menor (passivo tributário e multa) |
| Retenção de nota fiscal em trânsito | Painel OOH parado, atraso de obra, SLA de cliente quebrado |
| Autuação em fiscalização | Multa + juros + exposição da diretoria |
| Divergência entre CFOP e NCM | Nota fiscal rejeitada pelo destinatário |

### Como o K30-BOT Garante o NCM Correto

| Etapa | Mecanismo | Responsável |
|-------|-----------|-------------|
| 1. Classificação inicial | IA identifica categoria técnica do item (ex: "conector RJ45" → telecomunicação) | Gemini (IA) |
| 2. Sugestão de NCM | Bot consulta tabela interna de NCMs pré-validados pelo Fiscal, vinculada por categoria técnica | Base ISO 8000 + Firestore |
| 3. Nível de confiança | Se a IA tem certeza alta (>90%), aplica direto; se baixa, escala para aprovação humana | K30-BOT |
| 4. Validação humana (quando necessário) | Analista Fiscal confirma ou corrige o NCM antes do item ser liberado para compra | Departamento Fiscal |
| 5. Travamento na origem | Uma vez validado, o NCM fica vinculado ao código do item — não pode ser alterado sem novo workflow de aprovação | Governança K30 |
| 6. Versionamento | Se a legislação mudar, o NCM é atualizado centralmente e replicado a todos os itens da categoria (não item por item) | Fiscal + Consultor Tributário |

### Exemplo de Mapeamento Categoria → NCM → Tributação

| Categoria Técnica | Exemplo de Item | NCM Sugerido | Tributos Vinculados |
|--------------------|------------------|---------------|----------------------|
| Interface Visual | Monitor LCD 21.5" | 8528.59.20 | IPI, ICMS-ST (conforme UF), PIS/COFINS |
| Painel Elétrico/Automação | Modem celular 4G | 8517.62.55 | IPI, ICMS, PIS/COFINS |
| Estrutura Mecânica | Parafuso sextavado inox | 7318.15.00 | IPI reduzido, ICMS padrão |
| Conectividade | Cabo HDMI | 8544.42.00 | IPI, ICMS, PIS/COFINS |

> Esses NCMs são exemplos ilustrativos de referência; a tabela real será validada e homologada pelo time Fiscal da Eletromidia antes de qualquer cadastro em produção.

### Integração Fiscal com o SAP

| Recurso | O que Faz |
|---------|-----------|
| Vínculo automático com Tax Condition Type | O NCM sugerido já preenche o campo fiscal correspondente no cadastro de material do SAP |
| Checagem de consistência NCM x CFOP | Bot alerta se a combinação usada historicamente gerar divergência |
| Log de decisão fiscal | Cada NCM atribuído grava quem sugeriu (IA ou humano), quando e com qual nível de confiança |
| Reprocessamento em massa | Se um NCM de categoria for corrigido, todos os itens vinculados são atualizados automaticamente, sem retrabalho manual item a item |

### Papel do Departamento Fiscal no Processo

O Fiscal deixa de ser **corretor de erros depois do fato** e passa a ser **curador da base de conhecimento fiscal**:

- Homologa a tabela de NCMs por categoria técnica (uma vez, não item por item)
- Define o limiar de confiança da IA que exige revisão humana
- Recebe alertas automáticos quando a legislação de um NCM muda
- Tem acesso a relatório de auditoria com 100% dos NCMs atribuídos e sua justificativa

**Resultado direto:** o time Fiscal passa de um papel reativo (correção pós-cadastro) para um papel estratégico (governança da regra), com rastreabilidade total para qualquer fiscalização.

---

## 9. Rastreabilidade e Data Lineage

### O que é?

Um **histórico completo e imutável** de tudo que aconteceu com cada item — como uma "ficha criminal" que a Receita Federal pode auditar a qualquer momento.

### O que Registramos

| Informação | Para que Serve |
|------------|---------------|
| Quem cadastrou | Responsabilidade e governança |
| Quando cadastrou | Base para análise de obsolescência |
| Qual era o código antigo (De-Para) | Migração histórica sem perda de dados |
| Qual NCM foi atribuído | Comprovação fiscal |
| Quem aprovou | Workflow de governança |
| Histórico de alterações | Auditoria completa do ciclo de vida |

### Benefício Fiscal

> Em uma fiscalização da Receita Federal, a Eletromidia demonstra em segundos:
> - Por que aquele NCM foi escolhido
> - Quem validou a classificação
> - Quando foi feita
> - Qual a justificativa técnica

**Resultado:** Processo de defesa fiscal ágil e embasado.

---

## 10. Kitting e MRP: Da Teoria à Prática

### Kitting (Kits Padronizados)

**Conceito:** Antes do técnico sair do almoxarifado, ele recebe uma **caixa fechada** com TUDO que precisa para aquela ordem de serviço.

**Exemplo — Kit de Instalação de Painel OOH:**

| Item no Kit | Código ISO 8000 | Quantidade |
|-------------|----------------|------------|
| Parafuso sextavado M8x40 inox | `PARAFUSO, SEXTAVADO, M8X40, INOX` | 8 un |
| Bucha plástica 10mm | `BUCHA, PLASTICA, 10MM` | 8 un |
| Eletroduto galvanizado 1" | `ELETRODUTO, GALVANIZADO, 1 POL` | 2 m |
| Monitor LCD 21.5" Positivo | `MONITOR, LCD, 21.5 POLEGADAS, POSITIVO, MASTER` | 1 un |
| Modem celular Elsys Vivo 4G | `MODEM, CELULAR, ELSYS, VIVO, 4G + ANTENAS` | 1 un |
| Cabo HDMI 2m | `CABO, HDMI, 2 METROS, PREMIUM` | 2 un |

**Resultado:** Zero compras emergenciais. Zero idas extras a campo. Zero retrabalho.

### MRP (Reposição Inteligente)

**Conceito:** O SAP "sabe" quando reabastecer sozinho.

| Parâmetro | Configuração | Ação Automática |
|-----------|-------------|-----------------|
| Estoque mínimo | 20 unidades de parafuso M8x40 | Quando atinge 20, SAP gera requisição de compra |
| Lead time do fornecedor | 15 dias úteis | Pedido feito com antecedência |
| Ponto de ressuprimento | 30 unidades | Alerta amarelo no dashboard |
| Consumo médio mensal | 50 unidades | Projeção de necessidade |

**Resultado:** Nunca mais falta peça. Nunca mais compra de urgência.

---

## 11. Governança Contínua

### O Sistema Não Para de Evoluir

| Mecanismo | Frequência | Responsável | Objetivo |
|-----------|-----------|-------------|----------|
| Auditoria de saúde dos dados | Trimestral | Comitê multidisciplinar | Identificar cadastros fora do padrão |
| Treinamento de novos usuários | Mensal | Líder do Kaizen | Garantir que todos sabem usar o bot |
| Atualização de NCMs | Sempre que houver alteração legal | Fiscal + Consultor tributário | Manter compliance atualizado |
| Revisão de taxonomia | Semestral | Engenharia + Compras | Adaptar às novas tecnologias OOH |
| Métricas de qualidade de dados | Mensal | Governança de Dados | % de itens ISO 8000, % de NCMs corretos |

### Indicadores de Sucesso (KPIs)

| Indicador | Meta | Como Medir |
|-----------|------|------------|
| Itens cadastrados no padrão ISO 8000 | 100% | Relatório automático do bot |
| NCMs corretos na origem | 100% | Auditoria fiscal trimestral |
| Duplicidade de códigos | 0% | Script de validação diário |
| Compras fora do portal | 0% | Relatório de compras mensal |
| Tempo médio de cadastro | < 3 minutos | Log do bot |
| Taxa de retrabalho técnico em campo | < 2% | Ordens de serviço |

---

## 12. Resumo Executivo para Aprovação

### O que Estamos Propondo

| Item | Descrição |
|------|-----------|
| **O quê** | Chatbot inteligente no Slack para cadastro de itens |
| **Por quê** | Eliminar erros fiscais, duplicidades e compras emergenciais |
| **Como** | IA (Gemini) + Poka-Yoke + ISO 8000 + NCM automático |
| **Onde** | Google Cloud (seguro, escalável, certificado) |
| **Quando** | Implementação em fases — começando por itens de maior volume |
| **Quanto** | Infraestrutura sob demanda (paga-se apenas pelo uso) |

### Os 5 Motivos para Aprovar Agora

1. **Risco Fiscal Zero** — NCMs corretos desde o primeiro minuto, blindagem contra autuações
2. **Estoque Saneado** — Um código para cada item físico, eliminando duplicidades
3. **Compras Previsíveis** — Kitting + MRP = zero urgências, zero fretes extras
4. **Auditoria Imediata** — Data Lineage completo para qualquer fiscalização
5. **Padrão Internacional** — ISO 8000 e ISO 55000 posicionam a Eletromidia como referência no setor OOH

---

## 13. Próximos Passos

| Fase | Atividade | Prazo Estimado | Envolvidos |
|------|-----------|---------------|------------|
| 1 | Aprovação da arquitetura e orçamento | 1 semana | Patrocinador + Diretoria |
| 2 | Configuração do ambiente Google Cloud | 1 semana | TI + Consultor |
| 3 | Treinamento do modelo IA com taxonomia Eletromidia | 2 semanas | Engenharia + Fiscal + Compras |
| 4 | Piloto com 50 itens mais críticos | 2 semanas | Time multidisciplinar |
| 5 | Rollout completo da base | 1 mês | Todos os usuários |
| 6 | Governança contínua | Permanente | Comitê K30 |

---

> **"A qualidade dos dados é a qualidade da decisão."**
> 
> Com o K30-BOT, a Eletromidia garante que cada item cadastrado seja um ativo confiável — desde a entrada no sistema até a manutenção de campo.

---

**Projeto K30 — Kaizen Compras & Supply Chain**  
**Eletromidia — CLEVER SOLUÇÕES INTELIGENTES**