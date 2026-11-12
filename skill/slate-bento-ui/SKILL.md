---
name: slate-bento-ui
description: Cria interfaces, dashboards e web apps mobile-first no estilo Dark Slate Glassmorphic Bento UI, com cards modulares em ardósia escura, gráficos SVG interativos (sparklines bilaterais e anéis donut), tipografia tabular-nums e barra de navegação flutuante em vidro fosco.
---

# Skill: Dark Slate Glassmorphic Bento UI (`slate-bento-ui`)

Sempre que esta skill for ativada, construa a interface seguindo rigorosamente a arquitetura visual, a paleta cromática, os componentes modulares e as microinterações especificadas abaixo, adaptando apenas os dados, rótulos e ícones ao domínio solicitado pelo usuário.

---

## 1. Identidade Visual & Filosofia de Design

- **Estética Geral:** Telemetria de alta precisão inspirada em aplicativos nativos iOS modernos (dark mode profundo em azul-ardósia, superfícies em camadas sutis, cantos generosamente arredondados e controles em vidro fosco).
- **Hierarquia por Superfície (Sem Sombras Pesadas nos Cards):** Os cards não usam sombras projetadas pesadas; a separação visual ocorre pelo contraste entre o canvas `#0B1219`, o gradiente interno do card e uma borda hairline translúcida de `1px`.
- **Contenção Cromática:** Utilize exclusivamente a paleta definida nos tokens abaixo. Nunca introduza fundos preto puro (`#000000`), roxos genéricos ou cores saturadas fora dos acentos semânticos.

---

## 2. Design Tokens Obrigatórios (CSS)

Inclua e utilize exatamente estas variáveis CSS na raiz do projeto:

```css
:root {
  /* Superfícies & Fundos */
  --bg-canvas: #0B1219;
  --bg-card: linear-gradient(180deg, rgba(24, 37, 50, 0.92) 0%, rgba(17, 27, 37, 0.96) 100%);
  --bg-card-solid: #131E29;
  --bg-glass: rgba(22, 34, 48, 0.75);
  --bg-track: #1E2D3D;

  /* Bordas & Elevação */
  --border-card: 1px solid rgba(255, 255, 255, 0.07);
  --border-glass: 1px solid rgba(255, 255, 255, 0.13);
  --shadow-dock: 0 12px 32px rgba(0, 0, 0, 0.55);

  /* Tipografia */
  --font-sans: -apple-system, BlinkMacSystemFont, "SF Pro Display", "Inter", system-ui, sans-serif;
  --text-primary: #FFFFFF;
  --text-secondary: #F1F5F9;
  --text-muted: #6E8094;

  /* Acentos Semânticos & Gráficos */
  --accent-cyan: #38BDF8;
  --accent-cyan-soft: rgba(56, 189, 248, 0.15);
  --accent-amber: #F59E0B;
  --accent-gold: #FBBF24;
  --accent-coral: #F97316;
  --accent-orange-text: #FB923C;
  --accent-green: #34D399;
  --accent-green-soft: rgba(16, 185, 129, 0.15);
  --accent-slate: #475569;

  /* Geometria & Espaçamento */
  --radius-card: 20px;
  --radius-pill: 9999px;
  --card-padding: 16px 18px;
  --stack-gap: 12px;
}
```

---

## 3. Tipografia & Precisão Numérica

1. **Família Tipográfica:** `-apple-system, BlinkMacSystemFont, "SF Pro Display", "Inter", system-ui, sans-serif` com `-webkit-font-smoothing: antialiased`.
2. **Escala Hierárquica:**
   - **Título da Tela (Header):** `30px`, `font-weight: 700`, `letter-spacing: -0.02em`, cor `#FFFFFF`.
   - **Métrica Hero (Card Principal):** `28px` a `30px`, `font-weight: 700`, `letter-spacing: -0.025em`, cor `#FFFFFF`.
   - **Título de Card:** `16px`, `font-weight: 600`, `letter-spacing: -0.01em`, cor `#F8FAFC`.
   - **Valores de Status / Metadados:** `12.5px` a `13px`, `font-weight: 600`, cor `#F1F5F9`.
   - **Rótulos Muted / Eixos de Gráfico:** `10px` a `12px`, `font-weight: 500`, cor `#6E8094`.
3. **Alinhamento Tabular Obrigatório:** Todo número dinâmico, valor monetário, percentual, taxa ou eixo de gráfico **deve** receber:
   ```css
   font-variant-numeric: tabular-nums;
   font-feature-settings: "tnum" 1, "cv05" 1;
   ```

---

## 4. Estrutura de Layout & Cabeçalho

### Shell Responsivo (Mobile-First -> Bento Grid Desktop)
- **Mobile (`< 768px`):** Coluna única centralizada com `max-width: 440px`, `padding: 8px 16px 110px 16px` e `gap: 12px` entre cards. O `padding-bottom: 110px` é obrigatório para que o último card nunca fique encoberto pelo dock flutuante.
- **Tablet & Desktop (`>= 768px`):** Expande suavemente para `max-width: 1120px` com grid Bento de 2 colunas (`grid-template-columns: repeat(2, minmax(0, 1fr)); gap: 14px`).

### Cabeçalho Superior (Top Header)
- **Esquerda:** Título dinâmico da aba ativa (`<h1>` em `30px` bold).
- **Direita:** Botão circular de ação rápida (`40px x 40px`, `border-radius: 9999px`) em vidro fosco (`var(--bg-glass)`, `backdrop-filter: blur(18px) saturate(180%)`, `var(--border-glass)`) com ícone vetorial fino em `#38BDF8`. Ao clicar, alterna o contexto da tela (ex.: moeda, unidade ou período) e exibe uma pílula de feedback temporária em vidro fosco ao lado.

---

## 5. Os 4 Arquétipos de Cards Bento

Todo card segue a estrutura base: `background: var(--bg-card)`, `border: var(--border-card)`, `border-radius: var(--radius-card)`, `padding: 16px 18px`, com um cabeçalho interno (`margin-bottom: 14px`) contendo ícone vetorial (`18px`) + título à esquerda e uma **Pílula de Status** (`padding: 4px 10px`, `border-radius: 9999px`, `font-size: 12px`, `font-weight: 600`) ou alerta textual em `#FB923C` à direita.

### Arquétipo A — Métrica Hero + Resumo Empilhado
Ideal para KPIs principais (ex.: Saldo, Bateria, Patrimônio, Meta Diária).
- **Coluna Esquerda:** Número principal em destaque (`29px` bold) seguido de denominador muted (`14px` em `#6E8094`) e uma barra de progresso horizontal fina (`height: 5px`, `background: #1E2D3D`, preenchimento em gradiente).
- **Coluna Direita:** Pilha vertical de 2 a 3 linhas compactas (`gap: 9px`), cada uma com micro-ícone ou ponto colorido (`7px`) + rótulo muted à esquerda e valor alinhado à direita.

### Arquétipo B — Gráfico Sparkline Duplo com Eixos Y Bilaterais
Ideal para fluxos comparativos (ex.: Entradas × Saídas, Upload × Download, Dividendos × Aportes).
- **Resumo Superior:**
  - À esquerda: Duas métricas empilhadas em `16px` bold com setas direcionais — série primária (`↑`) em âmbar `#F59E0B` e série secundária (`↓`) em ciano `#38BDF8`.
  - À direita: Indicador de fonte ativa com ponto verde brilhante (`#34D399`) e mini-legenda das duas séries.
- **Área do Gráfico SVG (`height: 132px`):**
  - 5 linhas de grade horizontais tracejadas (`stroke="rgba(255,255,255,0.06)" stroke-dasharray="3,3"`).
  - Eixo Y esquerdo em ciano (`#38BDF8`, `10px`) e eixo Y direito em âmbar (`#F59E0B`, `10px`).
  - Curvas suaves (Bézier cúbica, `stroke-width: 1.8`) com preenchimento gradiente vertical sutil (`18%` de opacidade até `0%`).
  - **Interatividade:** Ao passar o cursor ou deslizar o dedo, exibir linha vertical guia tracejada, pontos destacados nas duas curvas e tooltip flutuante em vidro escuro no topo do gráfico.
- **Rodapé do Card:** Linha divisória sutil (`rgba(255,255,255,0.04)`) com pico da série ciano à esquerda (`↓ Pico ...`) e pico da série âmbar à direita (`↑ Pico ...`).

### Arquétipo C — Anel Donut Espesso + Legenda Segmentada + Histórico em Área
Ideal para composição proporcional + tendência histórica (ex.: Gastos por Categoria, Alocação de Carteira, Uso de CPU/Memória).
- **Metade Superior (Split Horizontal):**
  - **Esquerda:** Anel Donut SVG (`92px x 92px`, `stroke-width: 11`, fundo `#1E2D3D`) com espaçamento visual de `1.2%` entre fatias e valor central em destaque (`20px` bold + unidade `11px`).
  - **Direita:** Lista vertical de 4 a 5 categorias com ponto colorido (`7px`), nome em `#F1F5F9`, sub-valor opcional em `#6E8094` e percentual tabular à direita. Passar o mouse sobre uma fatia ou item destaca ambos e atualiza o número central do donut.
- **Metade Inferior (`height: 92px`, `margin-top: 12px`):**
  - Gráfico de área contínuo em ciano (`#38BDF8`) com gradiente suave, eixo Y esquerdo (`9.5px`), linha de referência horizontal tracejada marcada com `Avg` à direita e tooltip interativo ao passar o mouse/toque.

### Arquétipo D — Grid de Metadados `2xN`
Ideal para especificações técnicas, contas vinculadas ou ativos principais.
- Grid de 2 colunas (`grid-template-columns: repeat(2, minmax(0, 1fr))`, `gap: 12px 18px`).
- Cada célula exibe: micro-ícone colorido (`13px`) + chave em `#6E8094` (`12px`) à esquerda, e valor tabular em `#F1F5F9` (`12.5px` semibold) à direita.

---

## 6. Barra de Navegação Flutuante (Frosted Glass Pill Dock)

Implemente sempre uma barra de navegação flutuante em formato de pílula na base da tela para alternar entre as abas da aplicação:

- **Posicionamento:** `position: fixed; bottom: 20px; left: 50%; transform: translateX(-50%); z-index: 100;`
- **Dimensões:** `width: calc(100% - 48px); max-width: 340px; height: 58px; border-radius: 9999px; padding: 6px;`
- **Material Glassmorphism:**
  ```css
  background: rgba(22, 34, 48, 0.75);
  backdrop-filter: blur(22px) saturate(180%);
  -webkit-backdrop-filter: blur(22px) saturate(180%);
  border: 1px solid rgba(255, 255, 255, 0.13);
  box-shadow: 0 12px 32px rgba(0, 0, 0, 0.55);
  ```
- **Botões das Abas:**
  - **Estado Ativo:** Fundo pílula interno `rgba(255, 255, 255, 0.11)`, texto `#FFFFFF` e ícone vetorial em `#38BDF8`.
  - **Estado Inativo:** Fundo transparente e ícone/texto em `#6E8094`.

---

## 7. Checklist de Qualidade Técnica

- [ ] Todos os ícones e gráficos são SVG vetoriais inline (zero dependências externas quebráveis).
- [ ] Todos os valores numéricos usam `tabular-nums` para evitar trepidação visual ao atualizar dados.
- [ ] Todos os gráficos possuem inspeção interativa funcional tanto via mouse (`mousemove`) quanto via toque (`touchmove`).
- [ ] A troca de abas atualiza o título do cabeçalho, o estado visual do dock e renderiza suavemente o conjunto de cards correspondente.
