# IA com responsabilidade — skills para estudantes da UEM

Duas skills de agente (Claude Code, Claude.ai e outros agentes compatíveis com `SKILL.md`) que fazem a IA seguir o guia **[IA com responsabilidade](https://ppg.uem.br/arquivos-gerais/guia-ia_compressed.pdf)** da Universidade Estadual de Maringá, baseado na **Instrução Normativa GRE nº 1/2026**.

| Skill | Como dispara | O que faz |
|---|---|---|
| [`ia-responsavel-uem`](skills/ia-responsavel-uem/SKILL.md) | Sozinha, sempre que a IA apoiar uma atividade acadêmica | As regras do guia: semáforo verde/amarelo/vermelho, as 10 vedações do Art. 5º, o caminho Antes/Durante/Depois do Art. 7º, proteção de dados, verificação de referências, modelos de declaração de uso e checklist de bolso |
| [`tutor-uem`](skills/tutor-uem/SKILL.md) | Você digita `/tutor-uem <disciplina + tema>` | Um tutor para qualquer disciplina de graduação: a IA explica, dá exemplos e critica a sua tentativa; você resolve, escreve e defende |

> **Aviso.** Projeto pessoal de um estudante, **sem vínculo oficial com a UEM**. O guia não substitui o texto oficial da IN GRE nº 1/2026, e estas skills não substituem nenhum dos dois. A regra da sua disciplina ou do seu orientador vale primeiro. Dúvidas institucionais: sec-deg@uem.br · sec-ppg@uem.br.

## Por quê

Em 2026 a UEM publicou a IN GRE nº 1/2026 e um guia para estudantes explicando como usar IA na graduação e na pós. A ideia central do guia cabe numa frase: **a IA é apoio para aprender; o pensamento, as decisões e a autoria continuam seus.** Ele chega a dizer qual papel a IA deve ter durante uma atividade: o de **tutora**, que dá explicações, exemplos e críticas, e não resposta pronta (Art. 7º).

O problema é que um assistente de IA não sabe disso. Por padrão ele faz o que você pede: escreve a introdução do TCC, entrega a consulta SQL da lista valendo nota, processa a planilha com o nome e o RA dos colegas, inventa 15 referências com DOI e tudo. Cada uma dessas coisas é uma linha vermelha do guia.

Eu queria um tutor que eu pudesse usar em qualquer matéria sem ter que lembrar das regras a cada pedido, e que me ajudasse a aprender de verdade em vez de fazer por mim. Então transformei o guia em instruções que o agente segue:

- **Antes** de ajudar, ele pergunta se a atividade permite IA e confere se o material tem dados pessoais ou sigilosos.
- **Durante**, ele ensina: pede a sua tentativa primeiro, explica o que falta com exemplos paralelos (nunca os da atividade avaliativa) e corrige com dicas em degraus.
- **Depois**, ele aplica o teste da regra de ouro do guia — *se você não consegue explicar e defender cada parte sem a IA, ainda não é seu* — e, quando há entrega, rascunha a declaração de uso a partir do que realmente aconteceu na sessão.

As regras ficam numa skill separada (`ia-responsavel-uem`) para valerem também fora do tutor — num código de entrega, num resumo de artigo, numa revisão de texto — e para ficarem num lugar só: quando a norma mudar, muda um arquivo.

## Instalação

### Claude Code (plugin)

```bash
/plugin marketplace add Kelvin-Jesus/ia-responsavel-uem
/plugin install ia-responsavel-uem@ia-responsavel-uem
```

Instalado como plugin, o Claude Code põe o nome do plugin na frente das skills: o tutor fica em **`/ia-responsavel-uem:tutor-uem`**. Nas outras formas de instalação ele é só `/tutor-uem`. Para atualizar depois: `/plugin marketplace update ia-responsavel-uem`.

### Qualquer agente (skills.sh)

```bash
npx skills@latest add Kelvin-Jesus/ia-responsavel-uem
```

### Manual

Copie as pastas de `skills/` para `~/.claude/skills/` (todas as suas sessões) ou `.claude/skills/` (um projeto).

### Claude.ai

Compacte cada pasta de `skills/` num `.zip` e envie como skill personalizada nas configurações do Claude.ai. No `tutor-uem`, apague antes as linhas `disable-model-invocation` e `argument-hint` do cabeçalho: são campos do Claude Code que o Claude.ai não aceita.

## Como usar

Abra o agente na pasta onde você estuda a matéria e chame o tutor (pelo plugin, troque `/tutor-uem` por `/ia-responsavel-uem:tutor-uem`):

```text
/tutor-uem Estruturas de Dados — não entendo rotação em árvore AVL, tenho prova semana que vem
/tutor-uem Banco de Dados 2 — quero treinar divisão relacional
/tutor-uem Eng. de Software — o professor liberou IA no trabalho de ágil, só pediu para declarar
```

O tutor mantém na pasta:

| Arquivo | Para quê |
|---|---|
| `COMBINADOS.md` | O que cada professor permitiu (o registro que o Art. 8º pede) |
| `APRENDIZADO.md` | O que você domina, onde erra e quando revisar cada tópico |
| `FONTES.md` | Toda fonte citada, como `a verificar` até você ler o original |
| `registro/` | Um arquivo por sessão: os pedidos e o que cada lado fez. É o "guarde seus prompts" do guia e a base da declaração |

Preencher o `COMBINADOS.md` no começo do semestre evita que o tutor pergunte a permissão a cada atividade avaliativa.

## Como foi feito

- **Fonte única:** o PDF do guia, lido página por página. [`mapa-cobertura.md`](skills/ia-responsavel-uem/references/mapa-cobertura.md) liga cada uma das 20 seções ao lugar onde ela está nas skills, e todas as citações de artigo do guia foram conferidas por script contra as da skill. O texto é parafraseado com os números de artigo, para seguir o conteúdo do guia sem reproduzi-lo.
- **Escrita das skills:** princípios de [`writing-for-agents`](https://github.com/mattpocock/skills) do Matt Pocock — divulgação progressiva (detalhes em `references/`, lidos só quando o caso pede), critérios de conclusão claros em cada passo, uma fonte da verdade por regra, e a divisão entre skill acionada pelo modelo (regras) e skill acionada pelo usuário (tutor).
- **Processo e testes:** fluxo do [`skill-creator`](https://github.com/anthropics/skills) da Anthropic. Os casos de teste ficam em [`skills/tutor-uem/evals/`](skills/tutor-uem/evals/) (lista avaliativa sem permissão, dados de colegas, estudo de um tópico, referências com declaração) e rodam com `claude plugin eval`.
- **Pedagogia do tutor:** inspirada na skill `teach` do Matt Pocock (zona de desenvolvimento proximal, prática de recuperação, revisão espaçada), adaptada ao papel de tutora que o guia define.

## Créditos

- **Conteúdo normativo:** Universidade Estadual de Maringá — Pró-Reitoria de Ensino (PEN) e Pró-Reitoria de Pesquisa, Pós-Graduação e Inovação (PPG), autoras do guia *IA com responsabilidade*; comissão de elaboração da normativa instituída pela Portaria GRE nº 354/2026 (nomes na capa do guia). O guia e a IN pertencem à UEM; este repositório só os referencia.
- **Método de escrita de skills:** [Matt Pocock](https://github.com/mattpocock/skills) (`writing-for-agents`, `teach`), licença MIT.
- **Método de criação e avaliação:** [Anthropic `skill-creator`](https://github.com/anthropics/skills).
- **Autor:** [Kelvin Jesus](https://github.com/Kelvin-Jesus), estudante de Engenharia de Software.

## Como citar

Use o botão **"Cite this repository"** do GitHub (lê o [`CITATION.cff`](CITATION.cff)) ou:

> JESUS, Kelvin. **IA com responsabilidade: skills para estudantes da UEM**. 2026. Disponível em: https://github.com/Kelvin-Jesus/ia-responsavel-uem.

E cite também a fonte das regras:

> UNIVERSIDADE ESTADUAL DE MARINGÁ. **Instrução Normativa GRE nº 1/2026**. Dispõe sobre o uso responsável de ferramentas de Inteligência Artificial na UEM. Maringá: UEM, 2026.

## Contribuir

Achou uma regra do guia que a skill não segue, ou a IN mudou (ela pode ser revisada, Art. 12)? Abra uma issue citando a página do guia ou o artigo da IN. Toda mudança nas regras atualiza o `mapa-cobertura.md` junto.

## Declaração de uso de IA

Seguindo o Modelo 1 do próprio guia: este repositório foi elaborado com o apoio do Claude Opus 5.5 (Anthropic), em outubro de 2026, para organizar o conteúdo do guia em instruções para agentes, redigir as skills e este README. Revisei todo o conteúdo e conferi as regras contra o PDF do guia; as decisões de projeto são minhas.

## Licença

Código e texto das skills: [MIT](LICENSE). O guia *IA com responsabilidade* e a IN GRE nº 1/2026 são da UEM e não estão incluídos aqui.
