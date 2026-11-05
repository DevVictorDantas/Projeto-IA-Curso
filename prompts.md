# Empresta Aí — Registro de Prompts

Registro, em ordem cronológica, de todos os prompts usados para preencher a especificação ([espec.md](espec.md)).

---

## Prompt 01 — Contexto inicial do projeto

**Data:** 02/11/2026
**Seções afetadas:** nenhuma (configuração do trabalho)

> O projeto tem como objetivo inicial apenas completar o arquivo espec.md nos campos correspondentes sobre as especificações de um eventual sistema. O sistema se chama Empresta Aí, seria basicamente uma "copia" do Anota aí, no entanto, com a proposta de gerenciar o empréstimo de itens, como CD's, Video Games, livros e afins. Nesse caso, também será necessário criar um arquivo prompts.md onde serão documentados nesse arquivo todos os prompts que foram usados para completar esse trabalho. Lembrando que, esse trabalho inicial não vai lidar com código em momento nenhum, é apenas literalmente montar a ideia por escrito do eventual sistema a ser desenvolvido. Dito isso, as funcionalidades serão passadas por aqui, você preenche e automaticamente adiciona o prompt no prompt.md.

**Resultado:** criado este arquivo `prompts.md`. O `espec.md` ainda não foi alterado; aguardando as funcionalidades.

---

## Prompt 02 — Objetos, contatos, empréstimos e renovação

**Data:** 02/11/2026
**Seções afetadas:** cabeçalho (data), 1 a 12

> O sistema vai lidar com um crud dos objetos que serão emprestados, passando características como nome do objeto, categoria, possibilidade de incluir imagem sendo opcional e o estado de conservação.
>
> Também será necessário cadastrar os amigos/contatos para quem foi emprestado.
>
> O sistema vai contar com uma parte de registro de empréstimos, onde basicamente serão preenchidos os campos do primeiro ponto e ainda a data que foi emprestado e o prazo de devolução, de preferência em dias e mostrando também a data limite.
>
> Também será possível renovar um empréstimo. Aí já entra uma RN, só será possível renovar, caso o período de data após a finalização do  primeiro empréstimo estiver livre, ou seja, já não tiver sido "reservado" por outra pessoa.

**Resultado:** preenchidas visão, escopo, glossário, atores, regras de negócio RN01–RN20, histórias HU01–HU08 com critérios de aceite, estados do empréstimo, modelo de dados (Objeto, Contato, Empréstimo, Renovação, Reserva), casos de borda, requisitos não funcionais, decisões D01–D09 e questões em aberto Q01–Q05. A regra de renovação pedida virou a RN19.

---

## Prompt 03 — Devolução, não devolução, lista de empréstimos, avisos e histórico

**Data:** 02/11/2026
**Seções afetadas:** 1 a 12

> Continuando... O sistema também vai registrar a devolução, onde será informado o estado de conservação no retorno do objeto, bem como o registro da data.
>
> Em caso de perda ou não devolução, também ser possível registrar, colocando a data que deveria ter sido devolvido e o motivo.
>
> Ser possível listar os emprestimos ativos e nesses conseguir filtrar a situação de cada um, atrasado, encerrado e vigente.
>
> O dono deve ser informado por meio de avisos sobre prazos próximos, nesse caso EU estou estabelecimento 2 dias como sendo prazo próximo.
>
> O sistema deve constrar um histórico de objeto, ou seja, conseguir visualizar os emprestimos daquele objeto, quanto tempo ficou emprestado, para quem foi emprestado, se teve atraso, datas.

**Resultado:** adicionadas as regras RN21–RN40, as histórias HU09–HU13, os estados "Devolvido" e "Não devolvido", o status "Extraviado" do objeto, os campos de encerramento no modelo de dados, novos casos de borda e as decisões D10–D16. A questão Q01 (devolução) foi resolvida e removida; foi criada a Q06 (objeto extraviado que reaparece).

---

## Prompt 04 — Exibição do prazo, vencimento, tolerância, atrasos e alertas

**Data:** 02/11/2026
**Seções afetadas:** 1 a 12

> Algumas coisas fora de escopo:
>
> O prazo, conforme já dito, é mostrado em dias faltantes e a data limite, serão os 2 tipos ao mesmo tempo.
>
> A princípio não existe limite de prazo, no entanto, dependendo da necessidade de uso por parte do DONO, um aviso pode ser colocado em algum campo de observação e ser registrado a devolução alegando a necessidade.
>
> O prazo sempre vence das 23:59 do dia limite no fuso de Brasília.
>
>
> Quanto aos atrasos:
>
> O empresto consta como atrasado quando passa da data  e horário limite, desconsidarando o final de semana e feriados. Ou seja, se a data limite estava para sexta 23:59, a devolução pode ser feita sem atraso até a segunda, ou em caso de feriado, até o final do próximo dia útil depois do feriado.
>
> Quando estiver atrasado, não pode renovar e nem fazer novos empréstimos para aquela pessoa.
>
> O dono deve ser alertado todo final de dia que o empréstimo está atrasado e só param os avisos quando a devolução for feita.

**Resultado:** data limite e dias faltantes exibidos juntos (RN41); prazo e renovação sem limite máximo (RN13, RN18); observação no empréstimo e devolução "Solicitada pelo dono" com justificativa (RN46, RN47); vencimento às 23:59 de Brasília (RN42); dia útil e vencimento efetivo com tolerância de fim de semana e feriado nacional (RN43, RN44, RN50); atraso redefinido (RN16, RN25); atrasado não renova (RN17) e bloqueia novos empréstimos ao contato (RN45); alerta diário de atraso às 20:00 (RN48, RN49). Novas HU14 e entidade Feriado, HUs revisadas com datas reais de 2026, decisões D17–D25 (D04, D08 e D14 revisadas). Q04 e Q05 resolvidas e removidas; criada Q07 (feriados estaduais/municipais).

---

## Prompt 05 — Correções do primeiro feedback do Jhonne

**Data:** 05/11/2026
**Seções afetadas:** cabeçalho (data), 1 a 12

> Pontos a corrigir na v2:
>
> 1. *A RN19 depende de algo que não existe.* A renovação só é bloqueada por reserva, mas não há regra, história de usuário nem item de escopo para criar ou cancelar uma reserva. Isso aparece só como Q02. Hoje essa regra não tem como ser testada. Ou você especifica a reserva, ou tira a RN19 do escopo da v1.
> 2. *D15 contradiz RN48.* Os alertas são exibidos só dentro do app, mas o dono "recebe às 20:00", com uma janela de 20:00 a 20:05 nos requisitos não funcionais. Sem notificação push, o horário não significa nada. Isso precisa ser resolvido junto com a Q03.
> 3. *A anonimização da LGPD está incompleta.* Ao excluir o contato, o nome some, mas o motivo "Bruno perdeu o jogo na mudança" continua no histórico. Defina o que acontece com os textos livres.
> 4. *Falta um caso de borda nos empréstimos retroativos.* Nada impede registrar um empréstimo com data anterior à devolução do empréstimo anterior, o que cria períodos sobrepostos no histórico. A RN01 só verifica empréstimos em aberto.
> 5. *Pequenos detalhes:*
>    - A RN42 diz 23:59:59, enquanto o glossário e a RN44 dizem 23:59.
>    - O "hoje" das validações de data (RN12, RN23) não está fixado no horário de Brasília, ao contrário do vencimento.
>
> O Jhonne me passou 2 feedbacks, esse foi o primeiro. Faça um ajuste e me explique, qualquer coisa eu peço alteração.

**Resultado:** (1) reserva especificada: escopo, glossário, RN53–RN64, HU15 (registrar) e HU16 (cancelar), estados da reserva (Pendente, Atendida, Cancelada), modelo de dados e casos de borda; a RN19 passa a considerar só reservas pendentes; empréstimo para outro contato respeita a reserva (RN62); Q02 resolvida e removida. (2) App definido como mobile (Android e iOS); alerta de atraso vira notificação no celular às 20:00, também listado na tela inicial (RN48, RN66, RN67); avisos de prazo próximo ficam só na tela inicial (RN33); D15 revisada, D31 criada, Q03 reduzida à questão do armazenamento. (3) Exclusão de contato apaga também os textos livres dos empréstimos dele e as reservas dele (RN64, RN65); D16 e requisito de LGPD revisados. (4) Data do empréstimo não pode ser anterior à última devolução do objeto (RN52). (5) RN42 unificada em 23:59, com atraso a partir das 00:00; "hoje" fixado no horário de Brasília (RN51). Novas decisões D26–D33.

---

## Prompt 06 — Plataforma web e prazo de retirada da reserva

**Data:** 05/11/2026
**Seções afetadas:** 1, 2, 3, 5, 6, 7, 8, 9, 10, 11 e 12

> Sobre as decisões a tomar:
>
> 1 - Plataforma totalmente web, até pode funcionar o celular, mas a princípio é totalmente pelo Pc.
>
> 2- pode manter
>
> 3 - Reserva que não expira somente se não houver necessidade de devolução a pedido do dono ou algum pedido de empréstimo por outra pessoa. A princípio quem define é o dono o prazo, caso seja necessária a devolução.
>
> 4  - Certo.

**Resultado:** (1) plataforma trocada de celular para web, pensada para o computador; no celular funciona pelo navegador, sem layout dedicado. Alertas de atraso passam a ser notificações do navegador às 20:00 (RN48, RN66, RN67, HU14); dados em servidor com login do dono por e-mail e senha (D34), o que resolve e remove a Q03; D31 revisada. (2) Mantida uma reserva pendente por objeto (D27). (3) Reserva sem prazo continua sem expirar (RN68); quando o dono precisa do objeto ou outra pessoa pede emprestado, ele define um prazo de retirada com motivo (RN69–RN71) e, se não for atendida até lá, a reserva passa a "Expirada" (RN72, RN73). Nova HU17, novo estado Expirada, campos no modelo de dados, casos de borda, D28 revisada e D35 criada. (4) Data do cabeçalho mantida em 05/11/2026.

---

## Prompt 07 — Segundo feedback do Jhonne: sugestão de MVP

**Data:** 05/11/2026
**Seções afetadas:** nenhuma (apenas avaliação)

> ugestão de MVP — Empresta Aí
>
> *Proposta de valor mínima:* o dono sabe, ao abrir o app, onde está cada objeto, quem está atrasado e o que vence nos próximos dias. Tudo que não serve a isso fica para depois.
>
> ### O que entra no MVP
>
> | Bloco | HUs | Ajuste em relação à spec |
> | --- | --- | --- |
> | Objetos | HU01–HU04 | *Sem imagem* (RN05 fica para a v1.1) |
> | Contatos | HU05–HU06 | Nome e telefone; e-mail é opcional |
> | Empréstimo | HU07 | Integral, inclusive o bloqueio por atraso (RN45) e a observação (RN47) |
> | Vencimento e atraso | RN41–RN44, RN16, RN50 | Integral. *É o coração do produto* |
> | Renovação | HU08 | Entra *sem reserva*: RN17, RN18 e RN20. Sem nenhuma reserva cadastrada, a RN19 é sempre satisfeita. Ela fica dormente até a reserva existir |
> | Devolução | HU09 | Integral. "Solicitada pelo dono" é só um checkbox com texto, custo baixo |
> | Não devolvido | HU10 | Integral. Extraviado é definitivo no MVP (Q06 adiada) |
> | Lista e filtros | HU11 | Integral |
> | Avisos e atrasos | HU12 + HU14 adaptada | *Painel na tela inicial*, calculado ao abrir o app: "vencem em breve" e "atrasados há N dias". Sem disparo às 20:00 |
> | Histórico | HU13 | Integral. É uma consulta simples |
>
> ### O que fica de fora, e por quê
>
> - *Reserva (Q02):* ainda não está especificada. É a maior fonte de complexidade (conflitos de período, cancelamento, o que fazer quando a data chega). Ela é a candidata natural para a v2.
> - *Alertas às 20:00 e push:* dependem da Q03 e exigem scheduler e infraestrutura de notificação. O painel resolve 80% do problema com 10% do custo.
> - *Imagem:* traz storage, validação de formato e tamanho e redimensionamento. Não muda a decisão de ninguém no MVP.
> - *Feriados estaduais e municipais (Q07):* nacionais bastam.
>
> ### Decisões que precisam ser fechadas antes de codar
>
> 1. *Plataforma (Q03).* Sugiro web responsiva na mesma stack do curso: FastAPI + React/Vite. Ela roda no celular pelo navegador e adia a discussão sobre mobile nativo.
> 2. *Autenticação.* A spec presume um usuário único, mas um app web publicado expõe dados pessoais de terceiros (LGPD). O MVP precisa de pelo menos um login para a conta do dono. A spec hoje não cobre isso.
> 3. *Feriados.* Em vez de manter uma tabela manual, use a biblioteca holidays do Python, com country="BR". Ela calcula inclusive os feriados móveis e torna desnecessária a premissa de "cadastrar com um ano de antecedência".
> 4. *LGPD na exclusão de contato:* decidir o que acontece com os textos livres (ponto 3 do comentário anterior). O mínimo para o MVP é avisar na tela de exclusão que observações e motivos não são apagados.
>
> ### Principal risco técnico e como mitigar
>
> O risco está na lógica de datas, não no CRUD. Erros de fuso, de tolerância e de contagem de dias são silenciosos.
>
> Recomendo isolar o cálculo de vencimento efetivo, situação e dias de atraso em *funções puras, com testes escritos *antes da implementação. As tabelas que o Victor já fez viram os casos de teste diretamente:
>
> - os exemplos da RN44 na seção 7;
> - os casos de borda da seção 9;
> - os cenários datados das HUs.
>
> Para isso, o "agora" deve ser injetável nas funções, e não lido do relógio do sistema. É o exercício perfeito de spec executável.
>
> ### Roadmap sugerido
>
> - *v1.0 (MVP):* o escopo acima.
> - *v1.1:* imagem do objeto, filtro por categoria refinado, e Q06 (objeto extraviado que reaparece).
> - *v2.0:* reserva completa (ativa a RN19), alertas agendados e push conforme a Q03, e feriados locais se a Q07 pedir.
>
> Esse foi o segundo feedback do Jhonne. Apenas faça uma avalição e veja o que pode ser adaptado para que se torne um MVP.

**Resultado:** avaliação feita na conversa, sem alterar o `espec.md`. Pontos já resolvidos pelos Prompts 05 e 06 (reserva especificada, plataforma web, textos livres apagados na exclusão); concordância com o corte de imagem, reserva e notificação, com os feriados calculados e com o requisito de testabilidade; manutenção da decisão de uso principal no PC e da RN65; identificada a falta de história de login.

---

## Prompt 08 — Especificação reduzida ao MVP

**Data:** 05/11/2026
**Seções afetadas:** todas (documento reescrito)

> Eu prefiro apagar corrigindo essas orientações para o MVP. Uma vez que a especificação já tem quase 1000 linhas e pode ser que esteja ficando longa demais.

**Resultado:** `espec.md` reescrito como "Especificação do MVP (v1.0)", de cerca de 950 para cerca de 770 linhas. Removidos: imagem do objeto, reserva (regras, HU15–HU17, estados, entidade e casos de borda), notificação às 20:00 e a entidade Feriado. Adicionados: história e regras de acesso por login (HU01, RN01–RN04), entidade Conta, painel da tela inicial com "Vencem em breve" e "Atrasados" (RN44–RN49, HU13 e HU15), feriados nacionais calculados automaticamente (RN25), exemplo com feriado móvel, requisito de testabilidade, seção 2.3 com o roadmap (v1.1 e v2.0) e decisões D27–D30. A RN65 (textos livres apagados) foi mantida, agora como RN55. Regras, histórias e decisões foram **renumeradas em sequência** (RN01–RN56, HU01–HU15, D01–D30); as referências nos prompts anteriores usam a numeração antiga. Questões Q06 e Q07 saíram (adiadas no roadmap ou decididas); nova Q01 sobre Carnaval e Corpus Christi.

---

## Prompt 09 — Carnaval e Corpus Christi como feriado

**Data:** 05/11/2026
**Seções afetadas:** 3, 5, 7, 9, 10, 11 e 12

> Sobrou só uma, a Q01: Carnaval e Corpus Christi são pontos facultativos, e é preciso decidir se contam como dias não úteis. Até lá, contam como dias úteis.
>
> Pode considerar como feriado também

**Resultado:** segunda e terça de Carnaval e Corpus Christi passam a contar como dias não úteis na tolerância (RN25, glossário); a Quarta-feira de Cinzas continua útil. Novos exemplos de vencimento efetivo (sexta 13/02/2026 → quarta 18/02/2026; quarta 03/06/2026 → sexta 05/06/2026) na seção 7 e nos casos de borda; requisito de calendário e D20 revisados. Q01 resolvida; não há mais questões em aberto.

---

## Prompt 10 — Pontos facultativos como sugestão, decididos pelo dono

**Data:** 05/11/2026
**Seções afetadas:** 2, 3, 4, 5, 6, 7, 8, 9, 10 e 11

> Isso, a meu ver, entraria como sugestões, pois no final, quem vai decidir de fato é o DONO.

**Resultado:** Carnaval (segunda e terça), Quarta-feira de Cinzas e Corpus Christi passam a ser pontos facultativos configuráveis pelo dono (RN57). Toda conta começa com a sugestão do sistema: Carnaval e Corpus Christi marcados como não úteis, Cinzas como útil (RN58). Feriados nacionais continuam fixos. A mudança recalcula os empréstimos em aberto, e as devoluções já registradas mantêm os dias de atraso gravados (RN36 revisada, RN59, campo `dias_atraso_devolucao`). Nova HU16, campos de configuração na Conta, novos casos de borda, D20 revisada e D31 criada.
