# Token Usage Dashboard - Economia

Um Space (aplicativo) para o **Muse** que ajuda você a **não gastar a cota gratuita/semanal à toa**: ele mostra, de forma clara e grande, quanto da cota semanal já foi usado, quanto ainda resta, quando a cota reseta e se, no ritmo atual, ela aguenta até o reset. Tokens adicionais (de indicação) aparecem só como reserva secundária.

Além da cota, o painel traz uma aba **Sistema** com as especificações completas da máquina onde o seu Muse roda (medidas ao vivo) e uma aba **Skills** com todas as skills instaladas no seu Muse — abrir, copiar, editar, criar e enviar skills novas.

> **Privacidade e portabilidade:** este repositório **não contém nenhum dado pessoal, nenhuma leitura de cota, nenhum valor de máquina e nenhuma lista de skills de ninguém**. Todos os números que o aplicativo mostra são **medidos na hora, no ambiente de quem instalar**: a cota é lida das informações oficiais de uso do Muse daquela conta, as especificações são medidas na máquina daquela instalação e as skills são lidas das pastas de skills daquela instalação. Nada é fixo no código.

## Como instalar no seu Muse

1. Abra o **seu** Muse (no celular ou no computador) e comece uma conversa nova.
2. Cole o link deste repositório junto com o pedido do arquivo [`PROMPT.md`](PROMPT.md) — ou simplesmente diga: *"cria esse aplicativo para mim a partir deste repositório, seguindo o SPEC.md"*.
3. O seu Muse lê a especificação, constrói o aplicativo como um **Space** na **sua** conta e ele aparece na sua barra lateral, já buscando os **seus** dados (sua cota, sua máquina, suas skills).

Não há deploy, servidor externo nem chave de API: o aplicativo vive dentro do Muse de quem instala, como um Space fullstack (TypeScript) com banco de dados próprio, que nasce vazio e é alimentado pelas leituras do próprio ambiente.

## O que tem neste repositório

- [`SPEC.md`](SPEC.md) — especificação completa do aplicativo: abas, campos, cálculos, modelo de dados, regras de portabilidade e as lições de implementação (leia antes de reconstruir).
- [`PROMPT.md`](PROMPT.md) — prompt pronto para colar no seu Muse e pedir a construção.
- [`skill/slate-bento-ui/SKILL.md`](skill/slate-bento-ui/SKILL.md) — a skill de design **Dark Slate Glassmorphic Bento UI**, o sistema visual do aplicativo (canvas ardósia, cards bento, dock flutuante de vidro fosco). O SPEC manda aplicar esta skill no visual inteiro.

## Funcionalidades

- **Aba Cota semanal:** cota restante em destaque, barra com marcos de 50/70/85%, alertas coloridos nesses níveis, cota usada, dias até o reset, projeção de duração da cota, reserva de tokens adicionais, gráfico de consumo diário (14 dias), comparação semana atual × anterior e um **mapa de uso estilo GitHub** (um quadradinho por dia, intensidade = consumo do dia).
- **Atualização dos dados:** automática uma vez por dia + botão **Atualizar** que força uma leitura nova do uso oficial na hora.
- **Aba Sistema:** especificações medidas ao vivo a cada abertura — sistema operacional, kernel, CPU (modelo, núcleos, uso), memória, rede, tempo ligado, armazenamento total/usado/livre com a divisão entre arquivos do usuário e parte do sistema, e a lista de integrações instaladas (instalada ≠ conectada).
- **Aba Skills:** todas as skills do seu Muse (suas, editáveis; e as do sistema, somente leitura), com busca por palavra ou frase, resumo curto do objetivo de cada uma, abrir/ler o conteúdo inteiro, **copiar** a skill inteira, **editar e salvar** (as suas), **criar skill nova** e **enviar um arquivo .md** como skill nova.

## Observação honesta sobre os dados

O Muse informa o uso da cota em **porcentagem do limite**, não em contagem exata de tokens. Por isso o aplicativo trabalha em %; se você preencher nos Ajustes o total semanal em tokens, ele passa a mostrar também as quantidades estimadas a partir do %.
