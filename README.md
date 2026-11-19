# CCA-Q2 â€” Claude Certified Architect â€” Preparatorio Interativo

<div align="center">
  <img src="capa/logo.svg" alt="Arckhé AI" height="48" />
  <p><strong>Arckhé AI</strong></p>
</div>


Curso preparatorio para a certificacao Claude Certified Architect (CCA-F) com aprendizado progressivo e formatos interativos.

## Estrutura do Projeto

```
cca-q2/
â”œâ”€â”€ index.html                     # Landing page
â”œâ”€â”€ css/style.css                  # Design system (dark theme, Anthropic palette)
â”œâ”€â”€ mapa/index.html                # Mapa SVG interativo dos 5 dominios
â”œâ”€â”€ cenarios/
â”‚   â”œâ”€â”€ index.html                 # Hub dos 6 cenarios
â”‚   â””â”€â”€ cenario-1..6.html          # 6 simulacoes interativas (4 decisoes cada)
â”œâ”€â”€ flashcards/index.html          # 55 flashcards com repeticao espacada (SM-2)
â””â”€â”€ camadas/
    â”œâ”€â”€ index.html                 # Timeline das 5 camadas
    â”œâ”€â”€ camada-1/index.html        # Fundamentos
    â”œâ”€â”€ camada-2/index.html        # Estrutura
    â”œâ”€â”€ camada-3/index.html        # Confiabilidade
    â”œâ”€â”€ camada-4/index.html        # Escala
    â””â”€â”€ camada-5/index.html        # Simulado (quiz 15 questoes)
```

## Formatos de Aprendizado

| Formato | Descricao |
|---------|-----------|
| **Mapa Visual** | Mapa conceitual SVG interativo com conexoes entre os 5 dominios e 30 task statements |
| **Cenarios** | 6 simulacoes "choose your path" com 24 decisoes arquiteturais no total |
| **Flashcards** | 55 cards com repeticao espacada (SM-2) e progresso salvo em localStorage |
| **Camadas** | 5 camadas de complexidade crescente com checklists interativos |

## 5 Dominios do Exame

| Dominio | Peso |
|---------|------|
| Arquitetura Agentica e Orquestracao | 27% |
| Design de Ferramentas e Integracao MCP | 18% |
| Configuracao e Fluxos do Claude Code | 20% |
| Engenharia de Prompt e Saida Estruturada | 20% |
| Gerenciamento de Contexto e Confiabilidade | 15% |

## 5 Regras de Ouro

1. **Hooks > Prompts** â€” Imposicao programatica para compliance 100%
2. **Erros Estruturados Sempre** â€” Inclua o que quebrou, tentou e alternativas
3. **4-5 Ferramentas por Agente** â€” Menos opcoes = decisoes melhores
4. **Revisao Independente** â€” Segunda instancia percebe o que a primeira nao percebe
5. **Exemplos > Instrucoes** â€” 2-3 exemplos few-shot vencem paragrafos de regras

## Dados do Exame

- **Formato:** 60 questoes multipla escolha, 120 minutos, proctored
- **Nota minima:** 720 / 1.000
- **Custo:** US$ 99 (gratis para primeiros 5.000 do Partner Network)
- **Registro:** anthropic.skilljar.com

## Tecnologia

- HTML/CSS/JS puro (sem frameworks)
- Dark theme responsivo
- Progresso salvo em localStorage
- Google Fonts (Inter)

---

<div align="center">
  <img src="capa/logo.svg" alt="Arckhé AI" height="32" />
  <p><strong>Arckhé AI</strong></p>
</div>