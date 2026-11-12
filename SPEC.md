# SPEC — Token Usage Dashboard - Economia

Especificação completa para reconstruir o aplicativo como um **Space fullstack (TypeScript)** do Muse, com banco de dados próprio. Interface em português (pt-BR). Visual: aplicar integralmente a skill **Dark Slate Glassmorphic Bento UI** incluída neste repositório (`skill/slate-bento-ui/SKILL.md`) — tokens de cor, arquétipos de card, dock flutuante e checklist de qualidade vêm dela.

## 0. Regra de portabilidade (a mais importante)

**Nenhum valor de cota, de máquina ou de skills pode ser fixo no código.** Tudo é medido ao vivo no ambiente de quem instalar:

- **Cota:** lida das informações oficiais de uso do Muse da conta local (uso % do limite semanal gratuito ou do plano, instante do reset, saldo/uso de tokens adicionais de indicação). Na primeira execução, o painel semeia seu primeiro snapshot a partir de uma leitura real feita naquele momento.
- **Sistema:** especificações medidas no sistema operacional local a cada abertura da aba (nada de valores gravados: a máquina pode ser substituída e os valores mudam).
- **Skills:** lidas das pastas de skills da instalação local (a pasta de skills customizadas/editáveis do usuário e a pasta de skills do sistema, somente leitura, nos caminhos que o ambiente local definir).
- **Banco:** nasce vazio (exceto o primeiro snapshot de cota, da leitura inicial). Sem dados de demonstração.

## 1. Objetivo

Ajudar o usuário a **proteger a cota semanal** do Muse: ver de relance quanto resta, decidir se vale usar agora, e perceber dias de gasto alto. Tokens adicionais (indicação) são reserva secundária, sempre em segundo plano.

## 2. Estrutura — 3 abas

Navegação por **dock flutuante de vidro fosco** na base (conforme a skill de design), com o título da aba ativa no cabeçalho:

1. **Cota semanal**
2. **Sistema**
3. **Skills**

## 3. Aba Cota semanal

### 3.1 Card Hero (arquétipo A da skill de design)
- **Cota semanal restante** em destaque gigante: % (e quantidade, se o total semanal em tokens estiver configurado nos Ajustes).
- Barra de progresso fina com os **marcos de 50%, 70% e 85%** de uso.
- Pilha lateral de apoio: **cota usada (%)**, **reset** (data/hora e dias restantes), **projeção**.
- **Pílula de alerta** conforme o nível: normal (< 50%), **amarelo ≥ 50%**, **laranja ≥ 70%**, **vermelho ≥ 85%**, com mensagem clara (ex.: "Você já usou mais de 70% da cota gratuita. Considere economizar.").

### 3.2 Cálculos (sempre derivados dos snapshots, nunca fixos)
- `usada %` = última leitura oficial. `restante %` = 100 − usada.
- **Dias até o reset:** do instante atual até o `reset_at` da última leitura (arredondar para cima na exibição).
- **Consumo diário de um dia:** diferença entre a última leitura daquele dia e a última do dia anterior, dentro do mesmo ciclo semanal; no reset, o consumo do novo ciclo recomeça da leitura pós-reset. Dias sem medição são **"sem medição"**, nunca zero disfarçado.
- **Média diária:** média dos consumos diários medidos nos últimos 7 dias (informar internamente quantos dias entraram; a média alimenta a projeção e **não** precisa de card próprio).
- **Projeção:** `restante % ÷ média diária` = em quantos dias a cota acabaria no ritmo atual. Comparar com os dias até o reset para dizer se está seguro.
- **Ritmo seguro:** `restante % ÷ dias até o reset` = % por dia que pode gastar sem estourar (usado como série de comparação no gráfico de 14 dias).

### 3.3 Mapa de uso · estilo GitHub
- Grade de **um quadradinho por dia**: colunas = semanas, linhas = dias da semana (domingo no topo), cobrindo as **últimas 18 semanas**, com rótulos de mês e legenda "menos → mais".
- **Células perfeitamente quadradas (proporção 1:1) em qualquer tela.** ⚠️ Lição aprendida: regras globais de altura mínima de toque (ex.: 32px) podem vencer a largura das colunas no celular e transformar as células em retângulos verticais — as células do mapa devem ficar isentas dessa altura mínima e a grade não pode esticar as linhas.
- Cor = intensidade do consumo do dia, na escala verde do GitHub (cores de dados, não de decoração): sem medição = célula apagada; demais faixas por quartis do consumo (ex.: até 3%, até 7%, até 14%, acima de 14% do total semanal).
- Tocar/clicar num quadradinho mostra abaixo o **dia e o valor exato**, no formato `ddd., dd/mm — X% da cota`.
- Aviso honesto no card: o mapa enche um dia por vez a cada atualização diária; dias anteriores à instalação aparecem sem medição.

### 3.4 Gráfico de consumo diário · 14 dias (arquétipo B)
- SVG desenhado à mão, com inspeção por toque/mouse (linha guia, pontos e tooltip).
- Série 1 (ciano): consumo de cada dia. Série 2 (âmbar): ritmo seguro/dia. Resumo com as duas métricas e picos no rodapé.

### 3.5 Comparação semana atual × semana anterior
- % consumida no ciclo atual × % do ciclo anterior (quando houver leitura do ciclo anterior; sem dado, mostrar estado vazio honesto). Pode usar o arquétipo C inferior (área com linha de média).

### 3.6 Reserva de tokens adicionais
- Card secundário e discreto: tokens adicionais (indicação) **restantes e % usado**, lidos da mesma fonte oficial. Sem destaque visual de KPI principal.

### 3.7 Ajustes e atualização
- **Botão Atualizar:** dispara uma nova leitura do uso oficial (no ambiente local) e grava um novo snapshot; KPIs, gráfico e mapa derivam dele.
- **Atualização automática diária** (agendamento do próprio Space, de manhã cedo no fuso local): repete a leitura e grava o snapshot do dia.
- **Ajustes:** campo opcional `total semanal em tokens` — preenchido, o painel converte % em quantidades em todos os KPIs; vazio, tudo fica em %. (Meta diária opcional pode existir como configuração, sem card obrigatório.)

## 4. Aba Sistema

Tudo **medido ao vivo a cada abertura** (ação de leitura no servidor local). Seções:

- **Armazenamento** (arquétipo C, donut tocável): total do volume da home local, usado × livre, com a divisão **arquivos do usuário (workspace) × parte do sistema/assistente**, em GB e %.
- **CPU** (donut ou medidores): modelo do processador, núcleos visíveis, % de uso dividida em usuário/sistema/ocioso e carga média (load average).
- **Memória:** total, usada, livre/disponível e swap, com barra.
- **Rede:** taxas de download/upload por segundo (amostradas na abertura; podem vir 0 num instante ocioso — é medição real) e IP local.
- **Ambiente** (arquétipo D, grade de metadados 2×N): sistema operacional e versão, kernel, arquitetura, tempo ligado (uptime), fuso horário, modelo do assistente e sua janela de contexto, runtimes disponíveis (ex.: Python, Node) e contagens do ambiente (skills instaladas, Spaces criados).
- **Integrações instaladas:** lista das integrações/conectores disponíveis na instalação, com o aviso fixo: **instalada ≠ conectada** à conta.

## 5. Aba Skills

Fonte: as duas pastas de skills da instalação local.

- **Duas listas:** "Suas skills — você pode atualizar" (pasta customizada, editáveis) e "Skills do sistema" (somente leitura), cada uma com contagem.
- **Campo de busca no topo da aba:** filtra as duas listas ao digitar, por **palavra ou frase**, combinando nome + descrição + resumo, **ignorando maiúsculas e acentos**; mostra contagem de resultados, estado "nenhuma skill encontrada" e botão de limpar.
- **Card de cada skill:** nome em negrito; abaixo, um **resumo curto e completo do objetivo — para que a skill serve — em 1–2 frases, no máximo 2–3 linhas, sempre exibido por inteiro, sem corte com reticências** (não usar truncamento/line-clamp que corte a frase no meio); pílula de status (Editável / Somente leitura).
  - O resumo é **gerado a partir do conteúdo real** de cada arquivo de skill na primeira abertura e **cacheado** (chave = origem+id, invalidado pelo hash do conteúdo; regenerado se a skill for editada). Quase nenhuma skill traz descrição própria no arquivo — não depender dela.
- **Abrir skill:** mostra o conteúdo inteiro do arquivo.
- **Copiar skill:** botão na lista e na skill aberta; copia o conteúdo **inteiro** para a área de transferência, com confirmação visual ("Copiado ✓") e fallback (método alternativo) quando a Clipboard API for bloqueada no celular.
- **Editar skill (só as suas):** editor com o conteúdo; **Salvar** grava de volta no arquivo real da skill, na pasta customizada local. Skills do sistema não têm edição (são mantidas pelo sistema e substituídas em atualizações).
- **+ Nova skill:** formulário (nome + conteúdo, com opção de **enviar um arquivo .md** do dispositivo, que preenche o formulário); ao criar, grava um arquivo de skill novo na pasta customizada local e a skill entra na lista na hora. **Nome duplicado é recusado** com mensagem clara.

## 6. Modelo de dados (banco do Space)

- `quota_snapshots`: `id`, `captured_at`, `used_pct`, `reset_at`, `additional_used_pct`, `additional_total_tokens`, `additional_remaining_tokens`. Leituras idênticas consecutivas são deduplicadas.
- `settings`: `weekly_total_tokens` (nullable), `daily_goal_tokens` (nullable).
- `skill_summaries`: `skill_key` (origem+id), `summary`, `content_hash`.
- Ações do servidor (nomes livres, comportamento obrigatório): obter painel (último snapshot + derivados: 14 dias, série do mapa de 18 semanas, média, projeção, comparativo semanal), atualizar uso (nova leitura oficial), obter especificações do sistema, obter estatísticas de armazenamento, listar skills, ler skill, salvar skill, criar skill, obter/gerar resumos de skills, salvar ajustes.

## 7. Regras de honestidade dos dados

- O Muse informa uso em **% do limite**, não em tokens exatos: nunca inventar contagem de tokens; quantidades só derivadas do total configurado pelo próprio usuário nos Ajustes.
- Dia sem medição ≠ dia de consumo zero.
- Especificação que não puder ser medida no ambiente aparece como "não disponível", nunca como valor de exemplo.
- Nenhum texto do aplicativo cita nomes de pessoas, lugares, contas ou valores de qualquer instalação específica.
