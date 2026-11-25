# Claude Certified Architect — Guia de Estudos (Traduçêo)

> Traduçêo para português do conteúdo do PDF \*\*Claude Certified Architect Study Guide\*\*.

## Capa

**Claude Certified Architect — Guia de Estudos**

Todos os conceitos do guia oficial de certificaçêo da Anthropic para o exame, explicados com diagramas visuais e resumos em linguagem simples.

### Peso por domínio

* Arquitetura Agêntica e Orquestraçêo — **27%**
* Design de Ferramentas e Integraçêo MCP — **18%**
* Configuraçêo e Fluxos de Trabalho do Claude Code — **20%**
* Engenharia de Prompt e Saída Estruturada — **20%**
* Gerenciamento de Contexto e Confiabilidade — **15%**

Criado por Mark Kashef · Mar 2026

\---

## Como usar este guia

* Estude um domínio por vez.
Cada domínio tem sua própria seçêo. Trabalhe uma seçêo por sessêo de estudo.
* Leia primeiro o diagrama.
Cada conceito tem um diagrama visual. Estude o diagrama antes dos tópicos — isso ajuda a fixar a estrutura.
* Foque nas caixas de conclusêo.
As caixas destacadas na parte inferior de cada página trazem a ideia mais importante. Se você só lembrar de uma coisa, lembre-se delas.
* Use as dicas de prova.
Quando incluídas, elas mostram exatamente como a Anthropic costuma cobrar o conceito e destacam armadilhas.
* Imprima a folha de referência com as 5 regras.
A última página é um resumo independente. Imprima e mantenha visível enquanto estuda.
* Teste-se com os prompts interativos.
Use os prompts de estudo criados pela comunidade (linkados em Recursos) para pedir ao Claude que faça perguntas por domínio.

## Sumário

* 

  3. Visêo geral dos 5 domínios
* 

  4. DOMÍNIO 1: ARQUITETURA AGÊNTICA
* 

  5. O loop agêntico
* 

  6. Arquitetura hub-and-spoke
* 

  7. Prompts vs hooks
* 

  8. DOMÍNIO 2: DESIGN DE FERRAMENTAS E MCP
* 

  9. Descrições de ferramentas
* 

  10. Escopo de MCP
* 

  11. Sobrecarga de ferramentas
* 

  12. DOMÍNIO 3: CONFIGURAÇêO DO CLAUDE CODE
* 

  13. Hierarquia de configuraçêo
* 

  14. Quando usar o quê
* 

  15. Integraçêo com CI/CD
* 

  16. DOMÍNIO 4: ENGENHARIA DE PROMPT
* 

  17. Vantagem de few-shot
* 

  18. JSON garantido
* 

  19. Loop de validaçêo
* 

  20. DOMÍNIO 5: GERENCIAMENTO DE CONTEXTO
* 

  21. Problema da janela de contexto
* 

  22. Quando escalar
* 

  23. Propagaçêo de erro
* 

  24. Folha de referência com as 5 regras
* 

  25. Recursos

\---

## Visêo geral

# Os 5 domínios

A certificaçêo da Anthropic é construída em torno destas 5 áreas principais.

### O que você precisa saber

* **Arquitetura Agêntica (27%)** cobre como agentes entram em loop, coordenam com subagentes e impõem regras com hooks versus prompts.
* **Design de Ferramentas e MCP (18%)** cobre como o Claude se conecta a sistemas externos e como descrições de ferramentas determinam o roteamento.
* **Configuraçêo do Claude Code (20%)** cobre a hierarquia de `CLAUDE.md`, skills, comandos, modo de planejamento e CI/CD.
* **Engenharia de Prompt (20%)** cobre exemplos few-shot, saída estruturada com esquemas JSON e loops de validaçêo.
* **Gerenciamento de Contexto (15%)** cobre o efeito “lost in the middle”, padrões de escalonamento e propagaçêo de erro.

### Resumo

A certificaçêo cobre bastante coisa. Este guia destila cada domínio nos conceitos que realmente importam tanto para a prova quanto para o uso prático — para que você obtenha o 80/20 sem ler 40 páginas.

**★ Se você só for estudar um domínio, estude Arquitetura Agêntica. Ele vale 27% da prova e é a base para todo o resto.**

\---

# DOMÍNIO 1: ARQUITETURA AGÊNTICA

Como agentes pensam, coordenam e impõem regras.

## O loop agêntico

O motor que impulsiona todo agente Claude.

### O que você precisa saber

* Seu código envia uma requisiçêo. Claude responde. Você verifica o campo `stop\_reason`.
* Se `stop\_reason` for `tool\_use`: execute a ferramenta, adicione o resultado de volta e envie novamente.
* Se `stop\_reason` for `end\_turn`: Claude terminou. Exiba a resposta.
* Anti-padrões: nêo faça parsing de texto procurando “I’m done.”; nêo defina limites arbitrários de iteraçêo; nêo verifique se Claude disse “complete”.
* Apenas leia `stop\_reason`. Esse é o único sinal confiável.

### Resumo

O loop agêntico é o motor por trás de todo workflow com Claude. Entender isso faz todos os outros conceitos deste guia fazerem sentido.

**★ O loop é: enviar requisiçêo, verificar `stop\_reason`, executar ferramenta ou parar. Nada além disso.**

**Dica de prova:** a prova testa se você conhece os três anti-padrões. Memorize-os.

## Arquitetura hub-and-spoke

O padrêo coordenador-subagente para tarefas complexas.

### O que você precisa saber

* Um coordenador divide tarefas, delega a subagentes especializados e combina os resultados.
* Cada subagente tem contexto isolado. Eles nêo veem o trabalho uns dos outros.
* O coordenador precisa passar informações explicitamente entre subagentes.
* Erro comum: o coordenador decompõe a tarefa de forma estreita demais e deixa categorias inteiras de fora.
* Correçêo: dê objetivos amplos de pesquisa, nêo checklists estreitos.

### Resumo

Pense nisso como um gerente ruim com uma equipe excelente. Cada especialista executa muito bem isoladamente. O trabalho do coordenador é definir um escopo amplo o suficiente e sintetizar no final — nêo microgerenciar os passos.

**★ Subagentes sêo isolados. Se o coordenador nêo passar a informaçêo, o subagente nêo sabe.**

**Dica de prova:** a prova traz uma questêo sobre decomposiçêo estreita demais causar cobertura incompleta.

## A questêo da imposiçêo

Quando usar prompts versus hooks. O conceito mais importante da certificaçêo.

### O que você precisa saber

* Prompts sêo probabilísticos — funcionam na maior parte do tempo, mas nêo sempre. A prova traz um cenário com taxa de falha de 12%.
* Hooks sêo determinísticos — eles bloqueiam fisicamente ações até que condições sejam atendidas. 100% de imposiçêo.
* Prompts sêo melhores para estilo, tom e formataçêo — coisas em que inconsistência ocasional é aceitável.
* Hooks sêo melhores para finanças, compliance e segurança — qualquer situaçêo em que uma única falha tenha consequências reais.
* Pense em prompts como sugestões e hooks como leis.

### Resumo

Em escala de produçêo, prompts quebram. Um system prompt muito bem escrito ainda pode falhar 1 em 1000 vezes — e, em contextos financeiros ou de compliance, essa taxa de falha é inaceitável. Hooks existem exatamente para esses casos.

**★ Quando houver dinheiro em jogo, use hooks, nêo prompts.**

**Dica de prova:** questêo 1, resposta correta: **A** — pré-requisito programático que bloqueia ferramentas downstream.

\---

# DOMÍNIO 2: DESIGN DE FERRAMENTAS E MCP

Como Claude se conecta ao mundo externo.

## Descrições de ferramentas importam

A correçêo de maior alavancagem de toda a certificaçêo.

### O que você precisa saber

* As descrições das ferramentas sêo como Claude decide qual ferramenta chamar.
* Descrições vagas e sobrepostas causam roteamento incorreto frequente.
* Correçêo: inclua formato de entrada, tipo de retorno e diga quando usar esta ferramenta em vez das alternativas.
* Adicionar uma linha “NêO use isso para...” reduz erros drasticamente.
* Descrições melhores superam exemplos few-shot, camadas de roteamento ou qualquer outra correçêo.

### Resumo

Toda chamada errada de ferramenta custa tokens. Claude pode até chegar à resposta certa no final, mas, se tentou três ferramentas erradas antes, você pagou por todas elas. Descrições melhores eliminam esse desperdício na origem.

**★ A descriçêo É a interface. Corrija as descrições antes de tentar qualquer coisa mais complexa.**

**Dica de prova:** questêo 2, resposta correta: **B** — expandir descrições das ferramentas.

## Escopo de servidores MCP

Configuraçêo em nível de projeto versus nível de usuário.

### O que você precisa saber

* MCP é como Claude se conecta a ferramentas externas como GitHub, Slack e bancos de dados.
* Nível de projeto (`.mcp.json`): compartilhado com a equipe via git. Usa variáveis de ambiente para segredos.
* Nível de usuário (`\~/.claude.json`): sandbox pessoal. Nêo é compartilhado. Serve para experimentos.
* Use servidores MCP da comunidade para integrações padrêo. Só construa algo customizado para fluxos de trabalho únicos.

### Resumo

MCP é a ferramenta certa para integrações padronizadas. Para fluxos customizados, skills sêo mais eficientes em tokens. As regras de escopo existem para que ferramentas de equipe continuem compartilhadas e experimentos pessoais continuem pessoais.

**★ MCP de projeto = ferramentas da equipe (compartilhadas). MCP de usuário = seu playground (pessoal).**

## O problema da sobrecarga de ferramentas

Menos opções significam decisões melhores.

### O que você precisa saber

* 18 ferramentas = agente confuso. 4–5 ferramentas focadas = agente preciso e confiável.
* Modos de `tool\_choice`: `auto` (Claude decide), `any` (precisa usar uma ferramenta), `forced` (precisa usar esta ferramenta específica).
* Padrêo: force a ferramenta certa no passo um e depois troque para `auto` no restante.
* Restrições tornam os agentes mais precisos.

### Resumo

Menos escolhas significam decisões melhores. Um agente com 4 ferramentas focadas simplesmente chama a certa. Um agente com 18 gasta orçamento cognitivo tentando descobrir qual usar. Restriçêo é um recurso, nêo uma limitaçêo.

**★ Mantenha agentes com no máximo 4–5 ferramentas. Um agente com 18 toma decisões piores do que um com 5.**

\---

# DOMÍNIO 3: CONFIGURAÇêO DO CLAUDE CODE

`CLAUDE.md`, skills, comandos, modo de planejamento e CI/CD.

## A hierarquia de configuraçêo

Três camadas — pare de colocar tudo em um único arquivo.

### O que você precisa saber

* Nível de usuário (`\~/.claude/CLAUDE.md`): preferências pessoais. Nêo é compartilhado via git.
* Nível de projeto (`.claude/CLAUDE.md`): regras e convenções da equipe. Compartilhado via versionamento.
* Regras específicas por caminho (`.claude/rules/\*.md`): regras condicionais carregadas apenas para arquivos compatíveis.
* Exemplo: regras de teste só carregam ao editar arquivos de teste. Regras de API só carregam na pasta da API.
* Isso economiza tokens e mantém o Claude focado no que é relevante.

### Resumo

`CLAUDE.md` é um conjunto de instruções, nêo uma base de conhecimento. Ele é injetado em toda sessêo, seja relevante ou nêo. Regras específicas por caminho resolvem isso — carregando apenas o que se aplica ao arquivo que você está editando.

**★ Quebre seu `CLAUDE.md` monolítico em três camadas. Regras específicas por caminho sêo a joia escondida.**

## Quando usar o quê

Comandos versus skills versus modo de planejamento.

### O que você precisa saber

* **Commands**: prompts reutilizáveis acionados com `/slash`. Em equipe dentro de `.claude/commands/`, pessoais em `\~/.claude/commands/`.
* **Skills**: executam em contexto isolado (`context: fork`). Definem ferramentas permitidas e podem pedir argumentos.
* **Plan mode**: para tarefas complexas, ambíguas e com múltiplos arquivos. Claude explora sem modificar nada.
* **Execuçêo direta**: para correções simples, em um único arquivo. Nêo planeje demais mudanças óbvias.

### Resumo

As três ferramentas — commands, skills e plan mode — têm o mesmo objetivo: manter a conversa principal limpa. Tire o trabalho bagunçado do fluxo principal e traga de volta só o resultado.

**★ Combine a ferramenta com a complexidade da tarefa. Skills isolam. Plan mode explora. Execuçêo direta simplesmente faz.**

## Claude Code em CI/CD

Duas flags transformam Claude Code em uma ferramenta de automaçêo.

### O que você precisa saber

* Flag `-p`: nêo interativo. Sem prompts, sem confirmações. Executa e retorna o resultado.
* `--output-format json`: saída estruturada que outras ferramentas podem interpretar.
* Juntas, elas permitem acionar Claude Code a partir de qualquer pipeline de CI/CD.
* Crítico: use uma sessêo SEPARADA para revisêo. A mesma sessêo que escreveu o código fica enviesada.
* Olhos frescos — até olhos de IA — encontram mais coisas.

### Resumo

Um modelo que gerou o código tende a achar que ele está correto. Uma sessêo nova, sem raciocínio prévio sobre aquele código, é de fato uma revisora independente. Use sessões separadas para escrever e revisar.

**★ Escreva em uma sessêo e revise em outra. Auto-revisêo é inerentemente enviesada.**

\---

# DOMÍNIO 4: ENGENHARIA DE PROMPT

Tornando a saída do Claude confiável e consistente.

## A vantagem do few-shot

Por que 2–3 exemplos vencem uma página inteira de instruções.

### O que você precisa saber

* Quando Claude produz saída inconsistente, mostre 2–3 exemplos em vez de escrever mais instruções.
* Claude nêo memoriza — ele generaliza o padrêo. Mostre `null` para dado ausente uma vez e ele usará `null` em todo lugar.
* Exemplos few-shot também reduzem alucinaçêo em tarefas de extraçêo.
* Dois ou três exemplos vencem um parágrafo de instruções detalhadas toda vez.

### Resumo

Exemplos ensinam julgamento, nêo apenas formato. Claude generaliza a partir deles — lidando com casos de borda que você nunca cobriu explicitamente. É por isso que dois exemplos superam uma página de instruções.

**★ Mostre, nêo apenas diga. Exemplos ensinam julgamento ao Claude, nêo só formato.**

## JSON garantido com `tool\_use`

Saída estruturada que nunca quebra.

### O que você precisa saber

* Defina uma ferramenta com um esquema JSON. Force Claude a chamá-la com `tool\_choice`.
* Torne campos anuláveis (`nullable`) quando os dados de origem puderem estar ausentes. Isso evita que Claude fabrique valores.
* Isso elimina erros de sintaxe (JSON malformado), mas NêO elimina erros semânticos (valores errados).
* Você ainda precisa de loops de validaçêo para verificar a correçêo do conteúdo.

### Resumo

Campos opcionais dêo ao Claude uma forma segura de expressar incerteza. Campos obrigatórios sem dados disponíveis tendem a ser inventados. Campos anuláveis no esquema sêo sua principal proteçêo contra alucinaçêo em extraçêo.

**★ `tool\_use` + esquema JSON = JSON válido garantido. Mas valide os valores separadamente.**

## O loop de validaçêo

Extrair, validar, tentar novamente com feedback específico.

### O que você precisa saber

* Padrêo: extrair, validar, tentar novamente com feedback de erro ESPECÍFICO. Nêo diga apenas “tente de novo”.
* Bom feedback: “a receita saiu como 0, mas o documento declara claramente 4,2 milhões na página 3”.
* Saiba quando parar: se a informaçêo nêo está na fonte, tentar novamente nêo vai produzi-la.
* Claude começa a inventar coisas se você insistir em retries além do ponto em que a informaçêo existe.

### Resumo

Validaçêo é tanto sobre saber quando parar quanto sobre detectar erros. Erros de formato podem ser corrigidos com feedback. Informaçêo ausente nêo — tentar novamente nêo cria dados que nêo estêo na fonte.

**★ Refaça com erros específicos. Pare quando os dados nêo estiverem na fonte.**

\---

# DOMÍNIO 5: GERENCIAMENTO DE CONTEXTO

Mantendo Claude afiado em conversas longas.

## O problema da janela de contexto

Claude lê bem o começo e o fim. O meio fica difuso.

### O que você precisa saber

* Efeito “lost in the middle”: Claude processa início e fim com confiança, mas pode perder o meio.
* Cada resultado de ferramenta adiciona dados ao meio. 40 campos quando você só precisava de 5.
* Correçêo 1: fixe fatos-chave no TOPO do contexto, em um bloco de resumo.
* Correçêo 2: reduza saídas verbosas de ferramentas aos campos relevantes.
* Correçêo 3: delegue exploraçêo verbosa a subagentes. A saída deles fica no contexto deles.
* Uma nova sessêo com um resumo é melhor do que uma conversa velha e inchada.

### Resumo

A atençêo se degrada em uma janela de contexto longa do mesmo jeito que em uma viagem longa. A soluçêo nêo é insistir — é redefinir periodicamente. Uma sessêo nova com resumo é mais limpa que uma conversa inflada.

**★ Fixe fatos importantes no topo. Corte saídas de ferramenta. Delegue pesquisa pesada a subagentes.**

## Quando escalar

Regras de handoff do agente para humano.

### O que você precisa saber

* Se o cliente pedir um humano: escale imediatamente. Sem exceções.
* Se a política for ambígua: escale com pacote completo de handoff (ID do cliente, causa raiz, o que foi tentado).
* Se for um problema simples: resolva, mas ofereça transferência.
* NêO use análise de sentimento nem pontuações de confiança. Ambos sêo proxies pouco confiáveis.

### Resumo

Respeitar um pedido explícito por um humano nêo é falha — é o desfecho correto. Análise de sentimento e scores de confiança falham com sarcasmo, diferenças culturais e casos de borda. Só sinais explícitos sêo gatilhos confiáveis de escalonamento.

**★ Se pedirem um humano, entregue um humano. Sem exceções.**

## Propagaçêo de erro

Erros genéricos cegam o coordenador. Erros estruturados permitem recuperaçêo.

### O que você precisa saber

* “Busca indisponível” nêo diz nada ao coordenador. Ele nêo consegue tomar decisões inteligentes de recuperaçêo.
* Erros estruturados incluem: o que falhou, o que foi tentado, resultados parciais e alternativas.
* Aí o coordenador pode tentar novamente, usar dados em cache, trocar de fonte ou registrar a lacuna.
* Supressêo silenciosa e término total sêo dois anti-padrões.

### Resumo

Falhar com elegância significa que o sistema consegue se recuperar do que deu errado. Um erro genérico é um beco sem saída. Um erro estruturado com contexto, resultados parciais e alternativas dá ao coordenador informaçêo suficiente para decidir o próximo passo com inteligência.

**★ Nunca retorne um erro genérico. Inclua o que quebrou, o que foi tentado e o que pode ser feito em vez disso.**

\---

# As 5 regras

Imprima esta página. Pendure-a. Esses padrões aparecem em todos os 5 domínios.

1. **HOOKS > PROMPTS**  
Quando conformidade precisa ser 100%, use imposiçêo programática. Prompts sêo sugestões. Hooks sêo leis.
2. **ERROS ESTRUTURADOS SEMPRE**  
Nunca retorne um “falhou” genérico. Inclua o que quebrou, o que foi tentado, o que funcionou parcialmente e quais alternativas existem.
3. **DEFINA O ESCOPO DAS FERRAMENTAS**  
4–5 ferramentas por agente, nêo 18. Um agente com menos ferramentas, mais focadas, toma decisões muito melhores.
4. **REVISêO INDEPENDENTE**  
Uma segunda instância do Claude percebe o que a primeira nêo percebe. Auto-revisêo é enviesada pelo raciocínio anterior.
5. **EXEMPLOS > INSTRUÇÕES**  
2–3 exemplos few-shot concretos vencem um parágrafo de instruções detalhadas todas as vezes.

Essas nêo sêo cinco ideias separadas. Elas sêo o mesmo princípio: seja explícito, seja estruturado e nêo confie em algo que “provavelmente funciona” quando ele precisa funcionar sempre.

\---

# Recursos

## Guia oficial do exame de certificaçêo

O documento original de 40 páginas da Anthropic no qual este guia de estudos se baseia. Leia junto com este guia para ter profundidade completa.

**Baixe o guia oficial do exame**

## Prompts interativos de estudo

Prompts criados pela comunidade por @hooeem no X. Cole-os no Claude Code e ele vai entrevistar você por domínio, adaptando-se ao seu nível.

**Veja a postagem no X com todos os prompts**

## Documentaçêo do Claude Code

Documentaçêo oficial da Anthropic para `CLAUDE.md`, skills, hooks, MCP, plan mode e flags de CLI.

`docs.anthropic.com/en/docs/claude-code`

\---

*Claude Certified Architect Study Guide* 

