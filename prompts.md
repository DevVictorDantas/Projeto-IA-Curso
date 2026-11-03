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
