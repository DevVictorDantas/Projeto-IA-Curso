# Empresta Aí — Especificação v1

Aluno: Victor de Sá Dantas | Data: 02/11/2026

## 1. Visão

Quem empresta CDs, video games, livros e outros objetos para amigos costuma perder o controle de quem está com o quê e de quando o item deveria voltar. O Empresta Aí é um aplicativo pessoal, inspirado no Anota Aí, para o dono dos objetos registrar o seu acervo, os seus contatos e cada empréstimo feito, da saída até a devolução ou perda. Para cada empréstimo, o sistema mostra a data limite e os dias que faltam, considera fins de semana e feriados antes de marcar atraso, avisa quando o prazo está próximo e alerta todos os dias sobre empréstimos atrasados. O resultado esperado é que o dono saiba a qualquer momento onde está cada objeto, quem está atrasado e como foi o histórico de cada item.

## 2. Escopo

### 2.1 Dentro do escopo

- Cadastro, consulta, edição e exclusão (CRUD) de objetos: nome, categoria, imagem opcional e estado de conservação.
- Cadastro, consulta, edição e exclusão de contatos (amigos para quem os objetos são emprestados).
- Registro de empréstimo: objeto, contato, data do empréstimo, prazo em dias, observação opcional, e exibição da data limite junto com os dias faltantes.
- Vencimento às 23:59 do horário de Brasília, com tolerância de fim de semana e feriado nacional antes de considerar atraso.
- Renovação de empréstimo vigente, condicionada à ausência de reserva no período da renovação.
- Bloqueio de renovação e de novos empréstimos para contato com empréstimo atrasado.
- Registro de devolução, com data e estado de conservação no retorno, inclusive devolução antecipada por necessidade do dono.
- Registro de perda ou não devolução, com a data em que o objeto deveria ter sido devolvido e o motivo.
- Lista de empréstimos com filtro por situação: vigente, atrasado e encerrado.
- Avisos ao dono sobre prazos próximos (até 2 dias antes da data limite) e alertas diários de atraso.
- Histórico de cada objeto: empréstimos, contatos, datas, tempo emprestado e atrasos.

### 2.2 Fora do escopo

- Acesso dos contatos ao sistema (o contato não tem login; é apenas um registro do dono).
- Avisos ou alertas enviados aos contatos.
- Cobrança, multa ou qualquer transação financeira, inclusive por perda ou atraso.
- Limite máximo de prazo ou de quantidade de renovações.
- Empréstimo de um mesmo objeto para mais de uma pessoa ao mesmo tempo.
- Compartilhamento de acervo entre vários donos.

## 3. Glossário

- **Dono:** pessoa que usa o sistema e é proprietária dos objetos cadastrados.
- **Objeto:** item do dono que pode ser emprestado (CD, video game, livro etc.).
- **Categoria:** classificação do objeto, escolhida de uma lista (ver RN03).
- **Estado de conservação:** condição física do objeto, escolhida de uma escala fixa (ver RN04).
- **Contato:** pessoa para quem o dono empresta objetos. Não usa o sistema.
- **Empréstimo:** registro de que um objeto foi entregue a um contato em uma data, com um prazo de devolução.
- **Prazo:** quantidade de dias corridos que o contato tem para devolver o objeto, contados a partir da data do empréstimo.
- **Data limite:** último dia do prazo. É a data do empréstimo somada ao prazo, mais os dias de eventuais renovações. Vence às 23:59 do horário de Brasília.
- **Dias faltantes:** diferença em dias entre a data limite e hoje, exibida junto com a data limite.
- **Dia útil:** segunda a sexta-feira, exceto feriados nacionais.
- **Vencimento efetivo:** momento a partir do qual o empréstimo passa a ser atrasado. É a data limite às 23:59 ou, quando a data limite ou o dia seguinte a ela não forem dias úteis, as 23:59 do primeiro dia útil posterior à data limite (ver RN44).
- **Tolerância:** período entre o fim da data limite e o vencimento efetivo, em que a devolução ainda não é atraso.
- **Empréstimo ativo:** empréstimo ainda não encerrado e não atrasado, incluindo o período de tolerância. No filtro da lista aparece com o rótulo "Vigente".
- **Atrasado:** empréstimo ainda não encerrado cujo vencimento efetivo já passou.
- **Em aberto:** empréstimo ativo ou atrasado, ou seja, ainda não encerrado.
- **Devolvido:** empréstimo encerrado porque o objeto voltou ao dono.
- **Devolução com atraso:** devolução registrada com data posterior ao vencimento efetivo.
- **Devolução solicitada pelo dono:** devolução antes da data limite porque o dono precisou do objeto, registrada com justificativa.
- **Não devolvido:** empréstimo encerrado porque o objeto foi perdido ou o contato não vai devolvê-lo.
- **Encerrado:** empréstimo devolvido ou não devolvido.
- **Extraviado:** status do objeto cujo último empréstimo foi encerrado como não devolvido.
- **Renovação:** extensão da data limite de um empréstimo ativo por mais N dias.
- **Reserva:** registro de que um contato pretende pegar o objeto a partir de uma data. Ocupa o período a partir dessa data.
- **Prazo próximo:** situação de um empréstimo ativo a partir de 2 dias antes da data limite até o vencimento efetivo.
- **Aviso de prazo próximo:** mensagem exibida ao dono sobre um empréstimo com prazo próximo.
- **Alerta de atraso:** mensagem enviada ao dono todo fim de dia sobre cada empréstimo atrasado.
- **Histórico do objeto:** lista de todos os empréstimos de um objeto, em aberto e encerrados.

## 4. Atores e permissões

| Ator | Pode fazer | Não pode fazer |
| --- | --- | --- |
| Dono | Gerenciar objetos, contatos, empréstimos, renovações, devoluções, registros de não devolução e reservas do próprio acervo; ver avisos, alertas e históricos | Excluir objeto ou contato com empréstimo em aberto (RN06, RN08); renovar empréstimo atrasado ou com reserva no período (RN17, RN19); emprestar objeto extraviado (RN29); emprestar para contato com empréstimo atrasado (RN45) |
| Contato | Nada no sistema; aparece apenas como dado cadastrado pelo dono | Acessar, visualizar ou alterar qualquer informação; receber avisos |

## 5. Regras de negócio

Cada regra numerada, atômica e testável.

**Objetos**

- RN01 — Um objeto pode ter no máximo um empréstimo em aberto por vez.
- RN02 — O nome do objeto é obrigatório e tem de 1 a 100 caracteres, desconsiderando espaços nas pontas.
- RN03 — A categoria é obrigatória e deve ser uma das opções: CD, DVD/Blu-ray, Video game, Livro, Jogo de tabuleiro, Outros.
- RN04 — O estado de conservação é obrigatório e deve ser um dos valores: Novo, Ótimo, Bom, Regular, Danificado.
- RN05 — A imagem é opcional. Cada objeto tem no máximo uma imagem, nos formatos JPG, PNG ou WEBP, com até 5 MB.
- RN06 — Um objeto com empréstimo em aberto não pode ser excluído.
- RN07 — O status exibido do objeto é calculado: "Emprestado" se houver empréstimo em aberto; "Extraviado" se o último empréstimo foi encerrado como não devolvido; "Disponível" nos demais casos.

**Contatos**

- RN08 — Um contato com empréstimo em aberto não pode ser excluído.
- RN09 — O nome do contato é obrigatório (1 a 100 caracteres). Telefone e e-mail são opcionais, mas, se informados, devem ter formato válido.

**Empréstimos**

- RN10 — Para registrar um empréstimo, são obrigatórios: objeto, contato, data do empréstimo e prazo em dias.
- RN11 — Só pode ser emprestado um objeto com status "Disponível" (consequência da RN01 e da RN29).
- RN12 — A data do empréstimo vem preenchida com a data de hoje e não pode ser uma data futura.
- RN13 — O prazo é um número inteiro de pelo menos 1 dia, sem limite máximo.
- RN14 — A data limite é calculada como data do empréstimo + prazo em dias. O dono não a edita diretamente.
- RN15 — O estado de conservação do objeto no momento da saída fica gravado no empréstimo e não muda se o objeto for editado depois.
- RN45 — Um contato com qualquer empréstimo atrasado não pode receber novo empréstimo até que todos os seus atrasos sejam encerrados.
- RN47 — O empréstimo tem um campo de observação opcional (até 500 caracteres), editável enquanto estiver em aberto, por exemplo para anotar "vou precisar de volta no dia 10".

**Prazo, vencimento e atraso**

- RN41 — A data limite e os dias faltantes são sempre exibidos juntos: na tela de novo empréstimo (assim que o prazo é preenchido), na lista de empréstimos e no detalhe. Os dias faltantes são data limite − hoje; quando forem 0, o texto é "vence hoje".
- RN42 — Todo vencimento acontece às 23:59:59 do dia, no horário de Brasília (America/Sao_Paulo), independentemente do fuso do aparelho.
- RN43 — Dia útil é de segunda a sexta-feira, exceto feriados nacionais.
- RN44 — Se a data limite não for dia útil, ou se o dia seguinte à data limite não for dia útil, o vencimento efetivo passa para as 23:59 do primeiro dia útil posterior à data limite. Nos demais casos, o vencimento efetivo é a própria data limite às 23:59.
- RN16 — Um empréstimo é considerado atrasado quando o momento atual, no horário de Brasília, é posterior ao vencimento efetivo e ele ainda não foi encerrado.
- RN50 — Durante a tolerância, o empréstimo exibe "Prazo venceu em DD/MM/AAAA; devolução sem atraso até DD/MM/AAAA". Quando atrasado, exibe "Atrasado há N dias", em que N = hoje − data do vencimento efetivo.

**Renovação**

- RN17 — Só um empréstimo ativo pode ser renovado. Empréstimos atrasados ou encerrados não podem.
- RN18 — Na renovação, o dono informa N dias adicionais (N inteiro ≥ 1, sem limite máximo). A nova data limite é a data limite atual + N.
- RN19 — A renovação só é permitida se o período entre o dia seguinte à data limite atual e a nova data limite estiver livre, ou seja, se nenhuma reserva desse objeto tiver data de início nesse período.
- RN20 — Cada renovação fica registrada no histórico do empréstimo com a data da renovação, os dias adicionados, a data limite anterior e a nova.

**Devolução**

- RN21 — Só um empréstimo em aberto pode ter devolução registrada.
- RN22 — Para registrar a devolução, são obrigatórios a data de devolução e o estado de conservação no retorno (mesma escala da RN04).
- RN23 — A data de devolução vem preenchida com a data de hoje, não pode ser futura e não pode ser anterior à data do empréstimo.
- RN24 — Ao registrar a devolução, o estado de conservação do objeto passa a ser o estado informado no retorno.
- RN25 — Se a data de devolução for posterior à data do vencimento efetivo, a devolução é marcada "com atraso" e os dias de atraso são data de devolução − data do vencimento efetivo.
- RN46 — A devolução pode ser registrada antes da data limite. Se o dono marcar a opção "Solicitada pelo dono", a justificativa (5 a 500 caracteres) passa a ser obrigatória.

**Perda ou não devolução**

- RN26 — Só um empréstimo em aberto pode ser registrado como não devolvido.
- RN27 — Para registrar a não devolução, são obrigatórios a data em que o objeto deveria ter sido devolvido (vem preenchida com a data limite atual) e o motivo (texto de 5 a 500 caracteres).
- RN28 — A data em que o objeto deveria ter sido devolvido não pode ser anterior à data do empréstimo.
- RN29 — O registro de não devolução encerra o empréstimo e o objeto passa ao status "Extraviado", que não pode ser emprestado.

**Lista de empréstimos**

- RN30 — A situação de cada empréstimo, para fins de filtro, é uma das seguintes: Vigente (empréstimo ativo, inclusive em tolerância), Atrasado ou Encerrado (devolvido ou não devolvido).
- RN31 — A lista de empréstimos abre filtrada por Vigente e Atrasado, ordenada pela data limite da mais próxima para a mais distante. O dono pode marcar ou desmarcar cada situação, inclusive Encerrado.

**Avisos e alertas**

- RN32 — Um empréstimo está com prazo próximo quando está ativo e a data de hoje é igual ou posterior à data limite − 2 dias, até o vencimento efetivo.
- RN33 — Cada empréstimo com prazo próximo gera um aviso ao dono com objeto, contato, data limite e o texto "vence em 2 dias", "vence amanhã", "vence hoje" ou, na tolerância, o texto da RN50.
- RN34 — O aviso de prazo próximo deixa de ser exibido quando o empréstimo é encerrado, quando passa a atrasado ou quando é renovado para uma data limite fora da janela da RN32.
- RN48 — Todos os dias às 20:00 (horário de Brasília), o dono recebe um alerta para cada empréstimo atrasado, com objeto, contato e dias de atraso.
- RN49 — Os alertas de atraso de um empréstimo param apenas quando ele é encerrado pelo registro da devolução ou da não devolução.

**Histórico do objeto**

- RN35 — O histórico do objeto lista todos os empréstimos desse objeto, do mais recente para o mais antigo.
- RN36 — Cada item do histórico mostra: contato, data do empréstimo, data limite final, data de encerramento, situação, tempo emprestado, se houve atraso (e quantos dias), estado de conservação na saída e no retorno, renovações, observações e, se houver, a justificativa da devolução solicitada pelo dono ou o motivo da não devolução.
- RN37 — O tempo emprestado, em dias corridos, é a data de encerramento − data do empréstimo. Para empréstimo em aberto, usa-se a data de hoje. Para não devolvido, usa-se a data em que deveria ter sido devolvido.
- RN38 — "Houve atraso" é "Sim" quando o empréstimo foi devolvido com atraso (RN25) ou está atrasado (RN16); nos demais casos é "Não". Para não devolvido, o histórico mostra "Não devolvido" no lugar.
- RN39 — Ao excluir um contato sem empréstimo em aberto, os empréstimos encerrados dele continuam no histórico dos objetos, mas o contato aparece como "Contato removido".
- RN40 — Ao excluir um objeto, o histórico dele é excluído junto.

## 6. Histórias de usuário

Datas dos exemplos no calendário de 2026: 02/11 (segunda, feriado de Finados), 06/11 (sexta), 15/11 (domingo, feriado), 17/11 (terça), 19/11 (quinta), 20/11 (sexta, feriado da Consciência Negra), 23/11 (segunda).

### HU01 — Cadastrar objeto

Como dono, quero cadastrar um objeto, para poder emprestá-lo depois.
Regras relacionadas: RN02, RN03, RN04, RN05, RN07
Critérios de aceite:

- Cenário: cadastro válido sem imagem
  Dado que estou na tela de novo objeto
  Quando informo o nome "The Last of Us Part II", a categoria "Video game" e o estado "Ótimo"
  Então o objeto aparece na lista com status "Disponível" e sem imagem
- Cenário: cadastro válido com imagem
  Dado que estou na tela de novo objeto
  Quando informo nome, categoria e estado e anexo uma imagem PNG de 2 MB
  Então o objeto aparece na lista com a imagem anexada
- Cenário: nome vazio
  Dado que estou na tela de novo objeto
  Quando tento salvar sem nome
  Então vejo a mensagem "Informe o nome do objeto" e nada é salvo
- Cenário: estado de conservação não informado
  Dado que estou na tela de novo objeto
  Quando tento salvar sem escolher o estado de conservação
  Então vejo a mensagem "Informe o estado de conservação" e nada é salvo
- Cenário: imagem acima do limite
  Dado que estou na tela de novo objeto
  Quando anexo uma imagem de 8 MB
  Então vejo a mensagem "A imagem deve ter até 5 MB" e a imagem não é anexada

### HU02 — Consultar objetos

Como dono, quero ver a lista dos meus objetos, para saber o que tenho e o que está emprestado.
Regras relacionadas: RN07
Critérios de aceite:

- Cenário: lista com status
  Dado que tenho o objeto "Dom Casmurro" emprestado, o objeto "Abbey Road" sem empréstimo e o objeto "FIFA 23" registrado como não devolvido
  Quando abro a lista de objetos
  Então vejo "Dom Casmurro" com status "Emprestado", "Abbey Road" com status "Disponível" e "FIFA 23" com status "Extraviado"
- Cenário: filtro por categoria
  Dado que tenho objetos das categorias "Livro" e "CD"
  Quando filtro pela categoria "Livro"
  Então vejo apenas os objetos da categoria "Livro"

### HU03 — Editar objeto

Como dono, quero editar os dados de um objeto, para mantê-los corretos, por exemplo quando o estado de conservação muda.
Regras relacionadas: RN02, RN03, RN04, RN05, RN15
Critérios de aceite:

- Cenário: alterar estado de conservação
  Dado que o objeto "Abbey Road" tem estado "Bom"
  Quando altero o estado para "Regular" e salvo
  Então o objeto passa a exibir estado "Regular"
- Cenário: edição não altera empréstimos já feitos
  Dado que "Dom Casmurro" foi emprestado com estado "Ótimo"
  Quando altero o estado do objeto para "Danificado"
  Então o empréstimo continua mostrando o estado de saída "Ótimo"
- Cenário: remover imagem
  Dado que o objeto tem uma imagem
  Quando removo a imagem e salvo
  Então o objeto fica sem imagem

### HU04 — Excluir objeto

Como dono, quero excluir um objeto que não tenho mais, para manter o acervo atualizado.
Regras relacionadas: RN06, RN40
Critérios de aceite:

- Cenário: exclusão de objeto disponível
  Dado que o objeto "Abbey Road" está "Disponível"
  Quando escolho excluir e confirmo
  Então o objeto e o seu histórico deixam de aparecer
- Cenário: exclusão bloqueada por empréstimo em aberto
  Dado que o objeto "Dom Casmurro" está "Emprestado"
  Quando tento excluí-lo
  Então vejo a mensagem "Este objeto está emprestado e não pode ser excluído" e nada é excluído
- Cenário: exclusão de objeto extraviado
  Dado que o objeto "FIFA 23" está "Extraviado"
  Quando escolho excluir e confirmo
  Então o objeto deixa de aparecer na lista

### HU05 — Cadastrar contato

Como dono, quero cadastrar meus amigos como contatos, para registrar para quem empresto.
Regras relacionadas: RN09
Critérios de aceite:

- Cenário: cadastro válido
  Dado que estou na tela de novo contato
  Quando informo o nome "Ana Souza" e o telefone "(11) 98888-7777"
  Então o contato aparece na lista de contatos
- Cenário: nome vazio
  Dado que estou na tela de novo contato
  Quando tento salvar sem nome
  Então vejo a mensagem "Informe o nome do contato" e nada é salvo
- Cenário: e-mail inválido
  Dado que estou na tela de novo contato
  Quando informo o e-mail "ana@"
  Então vejo a mensagem "E-mail inválido" e nada é salvo

### HU06 — Editar e excluir contato

Como dono, quero editar ou excluir um contato, para manter minha lista correta.
Regras relacionadas: RN08, RN09, RN39
Critérios de aceite:

- Cenário: editar telefone
  Dado que o contato "Ana Souza" existe
  Quando altero o telefone e salvo
  Então o contato passa a exibir o novo telefone
- Cenário: exclusão bloqueada
  Dado que "Ana Souza" está com um objeto emprestado
  Quando tento excluí-la
  Então vejo a mensagem "Este contato tem empréstimo em aberto e não pode ser excluído" e nada é excluído
- Cenário: exclusão permitida com histórico
  Dado que "Ana Souza" não tem empréstimo em aberto e já devolveu "Abbey Road"
  Quando escolho excluir e confirmo
  Então o contato deixa de aparecer na lista
  E o histórico de "Abbey Road" mostra esse empréstimo com o contato "Contato removido"

### HU07 — Registrar empréstimo

Como dono, quero registrar que emprestei um objeto a um contato, informando o prazo em dias, para saber até quando ele deve ser devolvido.
Regras relacionadas: RN01, RN10, RN11, RN12, RN13, RN14, RN15, RN41, RN45, RN47
Critérios de aceite:

- Cenário: empréstimo válido com data limite e dias faltantes
  Dado que o objeto "Dom Casmurro" está "Disponível" e hoje é 02/11/2026
  Quando escolho o objeto, o contato "Ana Souza" e informo o prazo de 15 dias
  Então vejo "Data limite: 17/11/2026 — faltam 15 dias" antes de salvar
  E, ao salvar, o objeto passa a ter status "Emprestado"
- Cenário: prazo longo
  Dado que estou na tela de novo empréstimo
  Quando informo o prazo de 400 dias
  Então o empréstimo é aceito e a data limite é calculada normalmente
- Cenário: prazo inválido
  Dado que estou na tela de novo empréstimo
  Quando informo o prazo de 0 dias
  Então vejo a mensagem "O prazo deve ser de pelo menos 1 dia" e nada é salvo
- Cenário: objeto já emprestado ou extraviado
  Dado que "Dom Casmurro" já tem empréstimo em aberto e "FIFA 23" está "Extraviado"
  Quando abro a tela de novo empréstimo
  Então nenhum dos dois aparece entre os objetos que posso escolher
- Cenário: contato com empréstimo atrasado
  Dado que "Bruno Lima" tem um empréstimo atrasado
  Quando escolho "Bruno Lima" na tela de novo empréstimo
  Então vejo a mensagem "Bruno Lima tem empréstimo atrasado e não pode receber novos empréstimos" e não consigo salvar
- Cenário: observação no empréstimo
  Dado que estou registrando um empréstimo
  Quando preencho a observação "Vou precisar de volta no dia 10 para uma viagem"
  Então a observação aparece no detalhe do empréstimo
- Cenário: data futura
  Dado que hoje é 02/11/2026
  Quando informo a data do empréstimo 05/11/2026
  Então vejo a mensagem "A data do empréstimo não pode ser futura" e nada é salvo
- Cenário: empréstimo retroativo já atrasado
  Dado que hoje é 02/11/2026
  Quando informo a data do empréstimo 10/10/2026 e o prazo de 10 dias
  Então o empréstimo é salvo com data limite 20/10/2026 (terça, seguida de dia útil)
  E já aparece como "Atrasado há 13 dias"

### HU08 — Renovar empréstimo

Como dono, quero renovar um empréstimo por mais alguns dias, para que o contato fique mais tempo com o objeto sem atrapalhar quem já o reservou.
Regras relacionadas: RN17, RN18, RN19, RN20
Critérios de aceite:

- Cenário: renovação com período livre
  Dado que o empréstimo de "Dom Casmurro" tem data limite 17/11/2026 e não existe reserva do objeto
  Quando renovo por 10 dias
  Então a data limite passa a ser 27/11/2026
  E a renovação aparece no histórico do empréstimo
- Cenário: renovação bloqueada por reserva no período
  Dado que o empréstimo de "Dom Casmurro" tem data limite 17/11/2026
  E existe uma reserva de "Dom Casmurro" para "Bruno Lima" com início em 20/11/2026
  Quando tento renovar por 10 dias
  Então vejo a mensagem "Não é possível renovar: o objeto está reservado a partir de 20/11/2026" e a data limite continua 17/11/2026
- Cenário: reserva fora do período da renovação
  Dado que o empréstimo tem data limite 17/11/2026
  E existe uma reserva do objeto com início em 01/12/2026
  Quando renovo por 10 dias
  Então a data limite passa a ser 27/11/2026
- Cenário: renovação na tolerância
  Dado que o empréstimo tem data limite sexta 06/11/2026 e hoje é sábado 07/11/2026
  Quando renovo por 7 dias
  Então a data limite passa a ser 13/11/2026
- Cenário: empréstimo atrasado
  Dado que o empréstimo de "FIFA 23" está atrasado
  Quando abro esse empréstimo
  Então a opção de renovar não está disponível e vejo "Empréstimos atrasados não podem ser renovados"
- Cenário: empréstimo encerrado
  Dado que o empréstimo de "Abbey Road" está "Devolvido"
  Quando abro esse empréstimo
  Então a opção de renovar não está disponível

### HU09 — Registrar devolução

Como dono, quero registrar a devolução de um objeto com a data e o estado em que ele voltou, para encerrar o empréstimo e saber se o objeto voltou bem.
Regras relacionadas: RN21, RN22, RN23, RN24, RN25, RN44, RN46
Critérios de aceite:

- Cenário: devolução no prazo
  Dado que o empréstimo de "Dom Casmurro" tem data limite 17/11/2026 e estado de saída "Ótimo"
  Quando registro a devolução em 15/11/2026 com estado "Bom"
  Então o empréstimo fica "Devolvido", sem atraso
  E o objeto volta a "Disponível" com estado de conservação "Bom"
- Cenário: devolução com atraso
  Dado que o empréstimo de "Dom Casmurro" tem data limite terça 17/11/2026
  Quando registro a devolução em 20/11/2026
  Então o empréstimo fica "Devolvido" e marcado "com atraso de 3 dias"
- Cenário: data limite na sexta, devolução na segunda
  Dado que o empréstimo tem data limite sexta 06/11/2026
  Quando registro a devolução em segunda 09/11/2026
  Então o empréstimo fica "Devolvido", sem atraso
- Cenário: feriado logo após a data limite
  Dado que o empréstimo tem data limite quinta 19/11/2026 e sexta 20/11/2026 é feriado nacional
  Quando registro a devolução em segunda 23/11/2026
  Então o empréstimo fica "Devolvido", sem atraso
- Cenário: devolução antecipada solicitada pelo dono
  Dado que o empréstimo tem data limite 17/11/2026 e hoje é 08/11/2026
  Quando registro a devolução, marco "Solicitada pelo dono" e escrevo "Precisei do livro para um trabalho"
  Então o empréstimo fica "Devolvido" com a justificativa registrada
- Cenário: devolução solicitada sem justificativa
  Dado que estou na tela de devolução e marquei "Solicitada pelo dono"
  Quando tento salvar sem justificativa
  Então vejo a mensagem "Informe a justificativa" e nada é salvo
- Cenário: estado no retorno não informado
  Dado que estou na tela de devolução
  Quando tento salvar sem escolher o estado de conservação no retorno
  Então vejo a mensagem "Informe o estado de conservação no retorno" e nada é salvo
- Cenário: data de devolução anterior ao empréstimo
  Dado que o empréstimo foi feito em 02/11/2026
  Quando informo a data de devolução 01/11/2026
  Então vejo a mensagem "A devolução não pode ser anterior ao empréstimo" e nada é salvo

### HU10 — Registrar perda ou não devolução

Como dono, quero registrar que um objeto foi perdido ou não será devolvido, informando a data em que deveria ter voltado e o motivo, para encerrar o empréstimo e manter o registro do que aconteceu.
Regras relacionadas: RN26, RN27, RN28, RN29
Critérios de aceite:

- Cenário: registro válido
  Dado que o empréstimo de "FIFA 23" para "Bruno Lima" tem data limite 10/11/2026
  Quando escolho "Registrar perda/não devolução", mantenho a data 10/11/2026 e informo o motivo "Bruno perdeu o jogo na mudança"
  Então o empréstimo fica "Não devolvido"
  E o objeto "FIFA 23" passa ao status "Extraviado"
- Cenário: motivo vazio
  Dado que estou na tela de registro de não devolução
  Quando tento salvar sem motivo
  Então vejo a mensagem "Informe o motivo" e nada é salvo
- Cenário: data anterior ao empréstimo
  Dado que o empréstimo foi feito em 02/11/2026
  Quando informo que o objeto deveria ter sido devolvido em 30/10/2026
  Então vejo a mensagem "A data não pode ser anterior ao empréstimo" e nada é salvo

### HU11 — Listar e filtrar empréstimos

Como dono, quero ver os meus empréstimos e filtrá-los por situação, para saber rapidamente o que está vigente, atrasado ou encerrado.
Regras relacionadas: RN16, RN30, RN31, RN41, RN50
Critérios de aceite:

- Cenário: lista padrão com data limite e dias faltantes
  Dado que hoje é 02/11/2026 e tenho um empréstimo vigente com data limite 17/11/2026, um atrasado e um devolvido
  Quando abro a lista de empréstimos
  Então vejo o vigente com "17/11/2026 — faltam 15 dias" e o atrasado com "Atrasado há N dias", e não vejo o devolvido
- Cenário: empréstimo em tolerância
  Dado que hoje é sábado 07/11/2026 e um empréstimo tem data limite sexta 06/11/2026
  Quando abro a lista de empréstimos
  Então ele aparece como "Vigente" com o texto "Prazo venceu em 06/11/2026; devolução sem atraso até 09/11/2026"
- Cenário: filtrar só atrasados
  Dado que estou na lista de empréstimos
  Quando deixo marcado apenas o filtro "Atrasado"
  Então vejo somente os empréstimos cujo vencimento efetivo já passou e que não foram encerrados
- Cenário: filtrar encerrados
  Dado que tenho um empréstimo devolvido e um não devolvido
  Quando deixo marcado apenas o filtro "Encerrado"
  Então vejo os dois, cada um com a sua situação ("Devolvido" ou "Não devolvido")

### HU12 — Receber avisos de prazo próximo

Como dono, quero ser avisado quando um empréstimo estiver perto de vencer, para lembrar o contato antes que atrase.
Regras relacionadas: RN32, RN33, RN34
Critérios de aceite:

- Cenário: vence em 2 dias
  Dado que hoje é 15/11/2026 e o empréstimo de "Dom Casmurro" para "Ana Souza" tem data limite 17/11/2026
  Quando abro o sistema
  Então vejo o aviso "Dom Casmurro com Ana Souza vence em 2 dias (17/11/2026)"
- Cenário: vence hoje
  Dado que hoje é 17/11/2026 e a data limite é 17/11/2026
  Quando abro o sistema
  Então vejo o aviso com o texto "vence hoje"
- Cenário: aviso durante a tolerância
  Dado que hoje é domingo 08/11/2026 e a data limite foi sexta 06/11/2026
  Quando abro o sistema
  Então vejo o aviso "Prazo venceu em 06/11/2026; devolução sem atraso até 09/11/2026"
- Cenário: fora da janela
  Dado que hoje é 14/11/2026 e a data limite é 17/11/2026
  Quando abro o sistema
  Então não vejo aviso para esse empréstimo
- Cenário: aviso some após renovação
  Dado que existe aviso para o empréstimo com data limite 17/11/2026 e hoje é 15/11/2026
  Quando renovo o empréstimo por 10 dias
  Então o aviso deixa de ser exibido

### HU13 — Consultar histórico do objeto

Como dono, quero ver o histórico de empréstimos de um objeto, para saber com quem ele já esteve, por quanto tempo e se houve atraso.
Regras relacionadas: RN35, RN36, RN37, RN38
Critérios de aceite:

- Cenário: histórico com empréstimos encerrados e em aberto
  Dado que "Dom Casmurro" foi emprestado a "Bruno Lima" de 01/09/2026 a 10/09/2026 (data limite terça 08/09/2026) e hoje está com "Ana Souza" desde 02/11/2026
  Quando abro o histórico de "Dom Casmurro"
  Então vejo primeiro o empréstimo de "Ana Souza", em aberto
  E depois o de "Bruno Lima": 9 dias emprestado, houve atraso "Sim, 2 dias"
- Cenário: objeto sem empréstimos
  Dado que "Abbey Road" nunca foi emprestado
  Quando abro o histórico de "Abbey Road"
  Então vejo a mensagem "Este objeto ainda não foi emprestado"
- Cenário: empréstimo não devolvido no histórico
  Dado que "FIFA 23" foi registrado como não devolvido com o motivo "Bruno perdeu o jogo na mudança"
  Quando abro o histórico de "FIFA 23"
  Então vejo o empréstimo com situação "Não devolvido" e o motivo informado

### HU14 — Receber alertas de atraso

Como dono, quero ser alertado todo fim de dia sobre os empréstimos atrasados, para não esquecer de cobrar a devolução.
Regras relacionadas: RN16, RN44, RN48, RN49
Critérios de aceite:

- Cenário: primeiro alerta
  Dado que o empréstimo de "Dom Casmurro" com "Ana Souza" tem data limite terça 17/11/2026 e não foi devolvido
  Quando chegam as 20:00 de 18/11/2026
  Então recebo o alerta "Dom Casmurro com Ana Souza está atrasado há 1 dia"
- Cenário: alerta repetido diariamente
  Dado que o empréstimo continua atrasado
  Quando chegam as 20:00 de 19/11/2026
  Então recebo o alerta "Dom Casmurro com Ana Souza está atrasado há 2 dias"
- Cenário: sem alerta durante a tolerância
  Dado que a data limite foi sexta 06/11/2026
  Quando chegam as 20:00 de sábado 07/11/2026 e de domingo 08/11/2026
  Então não recebo alerta de atraso para esse empréstimo
- Cenário: alertas param com a devolução
  Dado que o empréstimo está atrasado
  Quando registro a devolução às 15:00 de 20/11/2026
  Então não recebo mais alertas desse empréstimo, inclusive às 20:00 do mesmo dia

## 7. Estados do empréstimo

Os estados Ativo e Atrasado são calculados a partir do momento atual (horário de Brasília) e do vencimento efetivo. Devolvido e Não devolvido são estados finais gravados pelo dono.

| Estado atual | Evento | Próximo estado | Regra |
| --- | --- | --- | --- |
| (nenhum) | Registrar empréstimo com vencimento efetivo ainda não passado | Ativo | RN10–RN14, RN44 |
| (nenhum) | Registrar empréstimo retroativo com vencimento efetivo já passado | Atrasado | RN12, RN16 |
| (nenhum) | Registrar empréstimo para contato com atraso | Recusado | RN45 |
| Ativo | Passam as 23:59 do vencimento efetivo sem encerramento | Atrasado | RN16, RN42, RN44 |
| Ativo | Renovar com período livre | Ativo (nova data limite) | RN17–RN20 |
| Ativo | Renovar com reserva no período | Sem mudança (renovação recusada) | RN19 |
| Atrasado | Renovar | Sem mudança (renovação recusada) | RN17 |
| Ativo | Registrar devolução | Devolvido | RN21–RN24 |
| Atrasado | Registrar devolução | Devolvido (com atraso) | RN21–RN25 |
| Ativo / Atrasado | Registrar perda ou não devolução | Não devolvido | RN26–RN29 |
| Devolvido / Não devolvido | Qualquer evento | Sem mudança (estado final) | RN17, RN21, RN26 |

Correspondência com o filtro da lista (RN30): Ativo → Vigente; Atrasado → Atrasado; Devolvido e Não devolvido → Encerrado.

Exemplos de vencimento efetivo (RN44):

| Data limite | Situação dos dias seguintes | Vencimento efetivo |
| --- | --- | --- |
| Terça 17/11/2026 | Quarta é dia útil | Terça 17/11/2026, 23:59 |
| Sexta 06/11/2026 | Sábado e domingo não são úteis | Segunda 09/11/2026, 23:59 |
| Quinta 19/11/2026 | Sexta 20/11 é feriado, depois fim de semana | Segunda 23/11/2026, 23:59 |
| Domingo 15/11/2026 | A própria data limite não é útil | Segunda 16/11/2026, 23:59 |

## 8. Modelo de dados

**Objeto**

| Atributo | Tipo | Obrigatório | Observação |
| --- | --- | --- | --- |
| id | identificador | sim | gerado pelo sistema |
| nome | texto (1–100) | sim | RN02 |
| categoria | enumeração | sim | RN03 |
| estado_conservacao | enumeração | sim | RN04; atualizado na devolução (RN24) |
| imagem | arquivo (JPG/PNG/WEBP, ≤ 5 MB) | não | RN05 |
| criado_em | data e hora | sim | gerado pelo sistema |
| atualizado_em | data e hora | sim | gerado pelo sistema |

O status (Disponível/Emprestado/Extraviado) não é gravado: é calculado pela RN07.

**Contato**

| Atributo | Tipo | Obrigatório | Observação |
| --- | --- | --- | --- |
| id | identificador | sim | gerado pelo sistema |
| nome | texto (1–100) | sim | RN09 |
| telefone | texto | não | formato validado se informado |
| email | texto | não | formato validado se informado |
| observacao | texto (até 500) | não | |

**Empréstimo**

| Atributo | Tipo | Obrigatório | Observação |
| --- | --- | --- | --- |
| id | identificador | sim | gerado pelo sistema |
| objeto_id | referência a Objeto | sim | |
| contato_id | referência a Contato | não | obrigatório no registro; fica vazio se o contato for excluído (RN39) |
| data_emprestimo | data | sim | RN12 |
| prazo_dias | inteiro (≥ 1) | sim | prazo original, RN13 |
| data_limite | data | sim | calculada (RN14), atualizada nas renovações (RN18) |
| estado_conservacao_saida | enumeração | sim | cópia do estado do objeto na saída, RN15 |
| observacao | texto (até 500) | não | editável enquanto em aberto, RN47 |
| encerramento | enumeração (Devolvido, Não devolvido) | não | vazio enquanto o empréstimo está em aberto |
| data_devolucao | data | não | obrigatória se encerramento = Devolvido (RN22, RN23) |
| estado_conservacao_retorno | enumeração | não | obrigatório se encerramento = Devolvido (RN22) |
| solicitada_pelo_dono | booleano | não | só na devolução, RN46 |
| justificativa_devolucao | texto (5–500) | não | obrigatória se solicitada_pelo_dono = sim (RN46) |
| data_prevista_devolucao | data | não | obrigatória se encerramento = Não devolvido (RN27, RN28) |
| motivo_nao_devolucao | texto (5–500) | não | obrigatório se encerramento = Não devolvido (RN27) |
| encerrado_em | data e hora | não | momento em que o dono registrou o encerramento |

Valores calculados, não gravados: vencimento efetivo (RN44), situação (se `encerramento` estiver preenchido, vale o seu valor; senão, Atrasado se o vencimento efetivo já passou; senão, Ativo), dias faltantes (RN41), dias de atraso (RN25, RN50), tempo emprestado (RN37) e prazo próximo (RN32).

**Renovação**

| Atributo | Tipo | Obrigatório | Observação |
| --- | --- | --- | --- |
| id | identificador | sim | |
| emprestimo_id | referência a Empréstimo | sim | |
| data_renovacao | data | sim | dia em que a renovação foi feita |
| dias_adicionais | inteiro (≥ 1) | sim | RN18 |
| data_limite_anterior | data | sim | RN20 |
| data_limite_nova | data | sim | RN20 |

**Reserva**

| Atributo | Tipo | Obrigatório | Observação |
| --- | --- | --- | --- |
| id | identificador | sim | |
| objeto_id | referência a Objeto | sim | |
| contato_id | referência a Contato | sim | quem reservou |
| data_inicio | data | sim | usada na RN19 |

**Feriado**

| Atributo | Tipo | Obrigatório | Observação |
| --- | --- | --- | --- |
| data | data | sim | única |
| nome | texto | sim | ex.: "Consciência Negra" |

Calendário de feriados nacionais usado pela RN43. Mantido pelo sistema, não pelo dono.

**Avisos, alertas e histórico:** não são entidades gravadas. Avisos de prazo próximo e alertas de atraso são calculados a partir dos empréstimos em aberto (RN32, RN48) e o histórico é a consulta dos empréstimos de um objeto (RN35).

**Relacionamentos**

- Objeto 1 — N Empréstimo (no máximo 1 em aberto, RN01)
- Contato 1 — N Empréstimo
- Empréstimo 1 — N Renovação
- Objeto 1 — N Reserva; Contato 1 — N Reserva

## 9. Casos de borda e erros

| Situação | Comportamento esperado | Regra |
| --- | --- | --- |
| Tentar emprestar objeto já emprestado ou extraviado | Objeto não aparece na lista de seleção; se o pedido chegar mesmo assim, é recusado | RN01, RN11, RN29 |
| Tentar emprestar para contato com empréstimo atrasado | Recusado com mensagem; o contato volta a poder receber empréstimos quando os atrasos forem encerrados | RN45 |
| Contato com empréstimo em tolerância (não atrasado) | Pode receber novo empréstimo normalmente | RN45, RN44 |
| Nome do objeto só com espaços | Tratado como vazio: "Informe o nome do objeto" | RN02 |
| Imagem em formato não aceito (ex.: GIF) | "Formato de imagem não suportado. Use JPG, PNG ou WEBP" | RN05 |
| Excluir objeto ou contato com empréstimo em aberto | Exclusão bloqueada com mensagem explicativa | RN06, RN08 |
| Prazo com decimais, negativo ou zero | "O prazo deve ser de pelo menos 1 dia" | RN13 |
| Prazo muito longo (ex.: 400 dias) | Aceito | RN13 |
| Data do empréstimo futura | "A data do empréstimo não pode ser futura" | RN12 |
| Empréstimo retroativo com vencimento efetivo já passado | Salvo e já exibido como "Atrasado" | RN12, RN16 |
| Aparelho do dono em outro fuso horário | Vencimento e atraso calculados no horário de Brasília | RN42 |
| Data limite na sexta | Sem atraso até segunda 23:59 | RN44 |
| Data limite na sexta e segunda é feriado | Sem atraso até terça 23:59 | RN44 |
| Data limite no sábado ou no domingo | Sem atraso até segunda 23:59 (ou o próximo dia útil) | RN44 |
| Data limite em dia útil seguido de dia útil | Atrasado a partir de 00:00 do dia seguinte | RN16, RN44 |
| Reserva começa exatamente no dia seguinte à data limite atual | Qualquer renovação é recusada, pois esse dia já faz parte do período da renovação | RN19 |
| Reserva começa exatamente na nova data limite | Renovação recusada | RN19 |
| Reserva começa no dia seguinte à nova data limite | Renovação permitida | RN19 |
| Renovar durante a tolerância | Permitido; a nova data limite parte da data limite atual | RN17, RN18 |
| Renovar empréstimo atrasado | Recusado: "Empréstimos atrasados não podem ser renovados" | RN17 |
| Renovar, devolver ou registrar perda de empréstimo encerrado | Opção indisponível | RN17, RN21, RN26 |
| Editar objeto emprestado | Permitido; o estado de saída gravado no empréstimo não muda | RN15 |
| Devolução no mesmo dia do empréstimo | Permitida; tempo emprestado = 0 dias | RN23, RN37 |
| Devolução exatamente no dia do vencimento efetivo | Não é atraso | RN25 |
| Devolução com data futura | "A data de devolução não pode ser futura" | RN23 |
| Devolução antes da data limite sem marcar "Solicitada pelo dono" | Aceita sem justificativa | RN46 |
| Objeto volta em estado pior que o da saída | Devolução aceita; o histórico mostra os dois estados | RN24, RN36 |
| Perda registrada antes da data limite (ex.: amigo avisou que perdeu) | Permitida; o dono pode trocar a data sugerida | RN27, RN28 |
| Motivo da não devolução com menos de 5 caracteres | "Informe o motivo (mínimo de 5 caracteres)" | RN27 |
| Empréstimo atrasado | Não gera aviso de prazo próximo; gera alerta diário às 20:00 e aparece no filtro "Atrasado" | RN34, RN48 |
| Vários empréstimos atrasados | Um alerta por empréstimo, todos os dias às 20:00 | RN48 |
| Vários empréstimos com prazo próximo | Um aviso por empréstimo, ordenados pela data limite | RN33 |
| Empréstimo atrasado registrado como não devolvido | Encerrado; os alertas param | RN49 |
| Contato excluído com empréstimos encerrados | Histórico mantido, exibindo "Contato removido" | RN39 |
| Nenhum empréstimo para os filtros escolhidos | "Nenhum empréstimo encontrado para os filtros selecionados" | RN31 |

## 10. Requisitos não funcionais

- **Desempenho:** a lista de objetos, a de empréstimos e o histórico de um objeto abrem em até 2 segundos com até 1.000 objetos e 5.000 empréstimos cadastrados.
- **Usabilidade:** a data limite e os dias faltantes aparecem juntos e na hora, enquanto o dono digita o prazo (RN41). Datas no formato DD/MM/AAAA. Avisos de prazo próximo e alertas de atraso aparecem em destaque na tela inicial, sem precisar abrir outra tela.
- **Acessibilidade:** campos com rótulos legíveis por leitor de tela, contraste mínimo WCAG 2.1 AA, e situações (Vigente/Atrasado/Encerrado), avisos e alertas indicados por texto, não só por cor ou ícone.
- **Privacidade (LGPD):** os contatos são dados pessoais de terceiros. O sistema guarda apenas o necessário (nome e, se o dono quiser, telefone e e-mail), não compartilha esses dados e permite excluir o contato quando não houver empréstimo em aberto. Na exclusão, os dados pessoais saem também do histórico (RN39). Observações, justificativas e motivos podem conter informação sobre o contato, por isso ficam visíveis só para o dono.
- **Armazenamento de imagens:** imagens limitadas a 5 MB (RN05) e redimensionadas para exibição na lista.
- **Horário de referência:** todos os cálculos de vencimento, atraso, prazo próximo e horário dos alertas usam o horário de Brasília (America/Sao_Paulo), independentemente do fuso do aparelho (RN42).
- **Calendário de feriados:** o calendário de feriados nacionais deve estar cadastrado com pelo menos um ano de antecedência (RN43).
- **Pontualidade dos alertas:** o alerta diário de atraso é emitido entre 20:00 e 20:05 do horário de Brasília (RN48).
- **Plataforma:** a definir (ver Q03).

## 11. Decisões e premissas

| ID | Decisão | Alternativas consideradas | Justificativa |
| --- | --- | --- | --- |
| D01 | O sistema tem um único usuário (o dono); contatos não acessam o sistema | Contatos com login próprio | Mantém o escopo de um app pessoal, como o Anota Aí |
| D02 | Categorias vêm de uma lista fixa | Categorias livres criadas pelo dono | Evita duplicidade ("Livro" x "Livros") e facilita o filtro |
| D03 | Estado de conservação em escala fixa de 5 níveis | Texto livre | Permite comparar o estado na saída e na volta |
| D04 | Prazo em dias corridos; a data limite é calculada e exibida junto com os dias faltantes | Mostrar só um dos dois; dono escolhe a data limite | Definição do dono: os dois formatos ao mesmo tempo |
| D05 | Status do objeto e situação do empréstimo são calculados; só o encerramento é gravado | Gravar status e atualizar por rotina | Evita inconsistência, por exemplo um "Atrasado" que não foi atualizado |
| D06 | Estado de conservação da saída fica gravado no empréstimo | Ler sempre o estado atual do objeto | Preserva o histórico mesmo que o objeto seja editado depois |
| D07 | Data do empréstimo pode ser retroativa, mas não futura | Só a data de hoje; permitir datas futuras | Retroativa permite registrar empréstimos já feitos; futura seria uma reserva |
| D08 | Empréstimo atrasado não pode ser renovado | Permitir renovar atrasados | Definição do dono |
| D09 | O período da renovação vai do dia seguinte à data limite atual até a nova data limite, inclusive | Considerar só a nova data limite | Interpretação de "período após a finalização do primeiro empréstimo estar livre" |
| D10 | Na devolução, o estado de conservação do objeto é atualizado para o estado no retorno | Manter o estado do cadastro e só registrar o de retorno | O próximo empréstimo já sai com o estado real do objeto |
| D11 | Perda e não devolução são um único tipo de encerramento ("Não devolvido"), com motivo em texto livre | Dois tipos separados (Perda / Não devolução) | Pedido trata os dois casos juntos; o motivo diferencia |
| D12 | A data em que o objeto deveria ter sido devolvido vem preenchida com a data limite atual, mas pode ser alterada | Campo vazio; usar sempre a data limite | Evita digitação no caso comum e permite registrar perda antecipada |
| D13 | A lista de empréstimos mostra todas as situações, mas abre filtrada em Vigente e Atrasado | Lista só de ativos; abrir com todos | O pedido cita "empréstimos ativos" e também o filtro "encerrado"; assim os dois atendem |
| D14 | O aviso de prazo próximo começa 2 dias antes da data limite e segue durante a tolerância | Encerrar o aviso na data limite | Definição do dono (2 dias); durante a tolerância o dono ainda precisa ser lembrado |
| D15 | Avisos e alertas são exibidos dentro do sistema, na tela inicial | Notificação push, e-mail | Funciona em qualquer plataforma; push depende da Q03 |
| D16 | Excluir contato mantém os empréstimos encerrados no histórico, sem os dados do contato | Bloquear exclusão de contato com histórico; apagar os empréstimos | Preserva o histórico do objeto e atende à LGPD |
| D17 | Sem limite máximo de prazo nem de quantidade de renovações | Limitar prazo (ex.: 365 dias) ou número de renovações | Definição do dono ("a princípio não existe limite de prazo") |
| D18 | Quando o dono precisa do objeto antes do prazo, ele anota na observação do empréstimo e registra a devolução marcando "Solicitada pelo dono", com justificativa | Criar um fluxo de "pedido de devolução" com novo prazo | Definição do dono; não exige regra nova de prazo |
| D19 | Vencimento às 23:59 do horário de Brasília, para qualquer aparelho | Usar o fuso do aparelho | Definição do dono; o mesmo empréstimo não fica atrasado em um aparelho e em dia no outro |
| D20 | Tolerância: se a data limite ou o dia seguinte não forem úteis, o vencimento vai para as 23:59 do primeiro dia útil posterior à data limite | Só adiar quando a própria data limite não é útil | Generaliza os exemplos do dono (sexta → segunda; feriado → próximo dia útil) e cobre data limite no sábado ou domingo |
| D21 | Apenas feriados nacionais contam como dia não útil | Incluir feriados estaduais e municipais | Calendário único e previsível; ver Q07 |
| D22 | Dias de atraso contam a partir do vencimento efetivo, não da data limite | Contar a partir da data limite | Se os dias de tolerância não são atraso, não devem entrar na contagem |
| D23 | O alerta de atraso é emitido às 20:00, horário de Brasília | 23:59; horário escolhido pelo dono | "Fim de dia" em um horário em que o dono ainda pode agir no mesmo dia |
| D24 | Os alertas de atraso param na devolução e também no registro de não devolução | Parar só na devolução | O registro de não devolução também encerra o empréstimo; continuar alertando não teria ação possível |
| D25 | O bloqueio de novos empréstimos vale para o contato com qualquer empréstimo atrasado e termina quando todos forem encerrados | Bloqueio por prazo fixo após a devolução | Definição do dono: o bloqueio dura enquanto houver atraso |

## 12. Questões em aberto

- Q02 — Como funciona a reserva: quem cria, como é cancelada e o que acontece quando chega a data de início com o objeto ainda emprestado? Um contato com empréstimo atrasado pode fazer reserva? (A reserva já aparece no modelo porque a RN19 depende dela.)
- Q03 — Qual a plataforma: app mobile, web ou ambos? Os dados ficam só no aparelho ou na nuvem? Se for mobile, avisos e alertas também devem ser notificações push?
- Q06 — Um objeto extraviado que reaparece pode voltar a "Disponível"? Se sim, como isso fica registrado no histórico?
- Q07 — Feriados estaduais e municipais da cidade do dono também devem contar para a tolerância?
