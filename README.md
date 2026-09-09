# ZOI DS Skill

Claude Code skill que força qualquer UI de projeto ZOI a seguir o design system oficial (`@zoitechnologies/ds`).

Sem ela, o agente inventa hex, tamanho de fonte e espaçamento. Com ela, todo valor visual sai de um token do DS.

## O que faz

- Checa se o projeto tem `@zoitechnologies/ds` instalado e wireado (theme.css + preset do Tailwind) antes de escrever UI
- Aplica 8 regras duras: nada de valor cru, semântico antes de primitivo, reuso de `.btn-*` / `.form-*` / `.card`, Clash Display nos títulos, verde como acento (não fundo), focus ring sempre, rem em vez de px, escala de spacing fechada
- Carrega a tabela completa de tokens (cores, spacing, radius, tipografia, motion, z-index, breakpoints) e os nomes de classe do preset Tailwind
- Serve de checklist de review: aponta hex hardcoded, token primitivo indevido, botão feito na mão, `outline: none` sem focus ring, spacing fora da escala

## Instalação

```bash
git clone https://github.com/raul-behnke/zoi-ds-skill.git ~/.claude/skills/zoi-ds
```

Ou, se preferir só o arquivo:

```bash
mkdir -p ~/.claude/skills/zoi-ds
curl -L https://raw.githubusercontent.com/raul-behnke/zoi-ds-skill/main/SKILL.md \
  -o ~/.claude/skills/zoi-ds/SKILL.md
```

Para valer só em um projeto, use `.claude/skills/zoi-ds/SKILL.md` dentro do repositório.

Reinicie o Claude Code e confirme com `/zoi-ds`.

## Uso

- `/zoi-ds` — invoca explicitamente
- Automático sempre que você pedir HTML/CSS/React/Vue em projeto ZOI
- Em review: "revise essa tela com o zoi-ds"

## Referência viva

O styleguide navegável vem junto com o pacote npm:

```
node_modules/@zoitechnologies/ds/styleguide/index.html
```

Abra no navegador — tem toggle light/dark.

## Limitações conhecidas

- `theme.css` só entrega light mode. Os overrides de `[data-theme="dark"]` existem apenas dentro do `styleguide/index.html`. Precisando de dark, copie os overrides de lá (ou mande PR pro pacote) — não invente valores novos.
- O pacote é só tokens + classes CSS. Não tem componente React/Vue; os componentes se constroem em cima dessas classes.

## Contribuindo

Mudou token ou regra no `@zoitechnologies/ds`? Atualize o `SKILL.md` aqui no mesmo PR — skill desatualizada é pior que skill nenhuma.
