# Empresta Aí — Especificação do MVP (v1.0)

Aluno: Victor de Sá Dantas | Data: 10/11/2026

## Resumo para avaliação

**O que é:** especificação, sem código, do MVP do Empresta Aí, um sistema web para o dono controlar os objetos que empresta. Tem 17 histórias de usuário (HU01–HU17), 62 regras de negócio (RN01–RN62), 33 decisões (D01–D33) e nenhuma questão em aberto. Todos os prompts usados estão em [prompts.md](prompts.md) (Prompts 01–14).

**Como foi construída:** as funcionalidades vieram dos Prompts 02 a 04. Depois vieram os dois feedbacks (Prompts 05 e 07), o corte para MVP (Prompt 08) e os ajustes finais do dono (Prompts 09 a 12).

**Primeiro feedback (pontos a corrigir):**

| Ponto | Como ficou |
| --- | --- |
| 1. RN19 dependia da reserva, que não estava especificada | A reserva chegou a ser especificada (Prompt 05), mas saiu do MVP e foi para a v2.0, junto com a renovação condicionada a ela (D29, seção 2.3) |
| 2. D15 contradizia a RN48 (alerta às 20:00 sem notificação) | A notificação foi para a v1.1; no MVP, avisos e atrasos ficam no painel da tela inicial (RN44–RN49, D14). Plataforma definida: web, uso principal no PC (D26) |
| 3. LGPD incompleta nos textos livres | Ao excluir o contato, observação, justificativa e motivo são apagados (RN55, D15) |
| 4. Empréstimo retroativo sobreposto | A data do empréstimo não pode ser anterior à última devolução do objeto (RN16, D24) |
| 5. 23:59 x 23:59:59 e "hoje" sem fuso | Tudo em 23:59, atraso a partir das 00:00 (RN24); "hoje" no horário de Brasília (RN22) |

**Segundo feedback (sugestão de MVP):** adotados o corte de imagem, reserva e notificação (D14, D28, D29), o painel na tela inicial, o login (HU01, RN01–RN04, D27), os feriados calculados automaticamente (RN25), o objeto "Extraviado" definitivo (D30), o requisito de testabilidade (seção 10) e o roadmap (seção 2.3). Pontos em que o dono decidiu diferente:

- Uso principal no computador, em vez de web responsiva (D26).
- Textos livres apagados na exclusão do contato, em vez de só avisar (D15).
- Carnaval, Quarta-feira de Cinzas e Corpus Christi são sugestões que o dono marca ou desmarca (RN57–RN59, D20, D31).

**Decisões finais do dono:**

- Só o empréstimo atrasado não pode ser renovado (RN29, D08).
- O dono pode excluir qualquer empréstimo registrado por engano (RN60–RN62, HU17, D32).
- A lista de objetos mostra com quem cada objeto está (RN10, D33).

**Onde olhar primeiro:** o risco principal está na lógica de datas. Ela está nas RN22–RN28 e na RN59, nos exemplos de vencimento efetivo da seção 7 e nos casos de borda da seção 9. Todos os exemplos com datas foram conferidos por script contra o calendário de 2026.

## 1. Visão

Quem empresta CDs, video games, livros e outros objetos para amigos costuma perder o controle de quem está com o quê e de quando o item deveria voltar. O Empresta Aí é um sistema web pessoal, usado principalmente no computador e inspirado no Anota Aí, para o dono dos objetos registrar o seu acervo, os seus contatos e cada empréstimo feito, da saída até a devolução ou perda. Para cada empréstimo, o sistema mostra a data limite e os dias que faltam e considera fins de semana e feriados antes de marcar atraso.

Proposta de valor do MVP: ao abrir o sistema, o dono sabe onde está cada objeto, quem está atrasado e o que vence nos próximos dias. O que não serve a isso fica para as próximas versões (seção 2.3).

## 2. Escopo

### 2.1 Dentro do escopo do MVP

- Criação de conta, entrada e saída do dono, com e-mail e senha.
- Cadastro, consulta, edição e exclusão (CRUD) de objetos: nome, categoria e estado de conservação. Objeto emprestado mostra com quem está e até quando.
- Cadastro, consulta, edição e exclusão de contatos (amigos para quem os objetos são emprestados).
- Registro de empréstimo: objeto, contato, data do empréstimo, prazo em dias e observação opcional, com a data limite exibida junto com os dias faltantes.
- Vencimento às 23:59 do horário de Brasília, com tolerância de fim de semana, feriado nacional e pontos facultativos escolhidos pelo dono antes de considerar atraso.
- Configuração, pelo dono, de quais pontos facultativos (Carnaval, Quarta-feira de Cinzas e Corpus Christi) contam como dias não úteis, a partir de uma sugestão do sistema.
- Renovação de empréstimo vigente.
- Bloqueio da renovação do empréstimo atrasado e de novos empréstimos para o contato que tem empréstimo atrasado; os outros empréstimos em dia desse contato continuam renováveis.
- Exclusão, com confirmação, de empréstimo registrado por engano, em aberto ou encerrado.
- Registro de devolução, com data e estado de conservação no retorno, inclusive devolução antecipada por necessidade do dono.
- Registro de perda ou não devolução, com a data em que o objeto deveria ter sido devolvido e o motivo.
- Lista de empréstimos com filtro por situação: vigente, atrasado e encerrado.
- Painel na tela inicial com os empréstimos que vencem em breve (até 2 dias antes da data limite) e os atrasados.
- Histórico de cada objeto: empréstimos, contatos, datas, tempo emprestado e atrasos.

### 2.2 Fora do escopo

- Acesso dos contatos ao sistema (o contato não tem conta; é apenas um registro do dono).
- Avisos ou alertas enviados aos contatos.
- Cobrança, multa ou qualquer transação financeira, inclusive por perda ou atraso.
- Limite máximo de prazo ou de quantidade de renovações.
- Empréstimo de um mesmo objeto para mais de uma pessoa ao mesmo tempo.
- Compartilhamento de acervo entre contas.
- Aplicativo nativo para celular. No celular, o sistema pode ser aberto pelo navegador, mas sem layout próprio para tela pequena.

### 2.3 Próximas versões (fora do MVP)

- **v1.1:** imagem do objeto; notificação do navegador às 20:00 para empréstimos atrasados; recuperação de senha por e-mail; objeto extraviado que reaparece e volta a ficar disponível.
- **v2.0:** reserva de objeto para um contato, com a renovação condicionada à ausência de reserva no período (regra pedida no Prompt 02); feriados estaduais e municipais, se forem necessários.

## 3. Glossário

- **Dono:** pessoa que usa o sistema e é proprietária dos objetos cadastrados.
- **Conta:** acesso do dono ao sistema, identificado por e-mail e senha. Cada conta tem o próprio acervo.
- **Objeto:** item do dono que pode ser emprestado (CD, video game, livro etc.).
- **Categoria:** classificação do objeto, escolhida de uma lista (ver RN07).
- **Estado de conservação:** condição física do objeto, escolhida de uma escala fixa (ver RN08).
- **Contato:** pessoa para quem o dono empresta objetos. Não usa o sistema.
- **Empréstimo:** registro de que um objeto foi entregue a um contato em uma data, com um prazo de devolução.
- **Prazo:** quantidade de dias corridos que o contato tem para devolver o objeto, contados a partir da data do empréstimo.
- **Hoje:** a data atual no horário de Brasília (America/Sao_Paulo), qualquer que seja o fuso do computador (ver RN22).
- **Data limite:** último dia do prazo. É a data do empréstimo somada ao prazo, mais os dias de eventuais renovações. Vence às 23:59 do horário de Brasília.
- **Dias faltantes:** diferença em dias entre a data limite e hoje, exibida junto com a data limite.
- **Dia útil:** segunda a sexta-feira, exceto feriados nacionais e os pontos facultativos que o dono marcou como não úteis.
- **Ponto facultativo:** data que não é feriado nacional, mas em que muita gente não trabalha. No sistema são três: Carnaval (segunda e terça), Quarta-feira de Cinzas e Corpus Christi.
- **Vencimento efetivo:** momento a partir do qual o empréstimo passa a ser atrasado. É a data limite às 23:59 ou, quando a data limite ou o dia seguinte a ela não forem dias úteis, as 23:59 do primeiro dia útil posterior à data limite (ver RN26).
- **Tolerância:** período entre o fim da data limite e o vencimento efetivo, em que a devolução ainda não é atraso.
- **Empréstimo ativo:** empréstimo ainda não encerrado e não atrasado, incluindo o período de tolerância. No filtro da lista aparece com o rótulo "Vigente".
- **Atrasado:** empréstimo ainda não encerrado cujo vencimento efetivo já passou.
- **Em aberto:** empréstimo ativo ou atrasado, ou seja, ainda não encerrado.
- **Devolvido:** empréstimo encerrado porque o objeto voltou ao dono.
- **Devolução com atraso:** devolução registrada com data posterior ao vencimento efetivo.
- **Devolução solicitada pelo dono:** devolução antes da data limite porque o dono precisou do objeto, registrada com justificativa.
- **Não devolvido:** empréstimo encerrado porque o objeto foi perdido ou o contato não vai devolvê-lo.
- **Encerrado:** empréstimo devolvido ou não devolvido.
- **Extraviado:** status do objeto cujo último empréstimo foi encerrado como não devolvido. No MVP é definitivo.
- **Renovação:** extensão da data limite de um empréstimo ativo por mais N dias.
- **Painel:** área da tela inicial com duas seções, "Vencem em breve" e "Atrasados".
- **Prazo próximo:** situação de um empréstimo ativo a partir de 2 dias antes da data limite até o vencimento efetivo.
- **Aviso de prazo próximo:** item da seção "Vencem em breve" do painel.
- **Alerta de atraso:** item da seção "Atrasados" do painel.
- **Histórico do objeto:** lista de todos os empréstimos de um objeto, em aberto e encerrados.

## 4. Atores e permissões

| Ator | Pode fazer | Não pode fazer |
| --- | --- | --- |
| Visitante (sem login) | Criar conta e entrar | Ver ou alterar qualquer dado (RN01) |
| Dono (com login) | Gerenciar objetos, contatos, empréstimos, renovações, devoluções e registros de não devolução da própria conta; ver o painel e os históricos; escolher os pontos facultativos (RN57); excluir empréstimo registrado por engano (RN60); sair | Ver dados de outra conta (RN03); excluir objeto ou contato com empréstimo em aberto (RN09, RN11); renovar empréstimo atrasado (RN29); emprestar objeto extraviado (RN41); emprestar para contato com empréstimo atrasado (RN20); registrar empréstimo retroativo sobreposto a um empréstimo anterior (RN16) |
| Contato | Nada no sistema; aparece apenas como dado cadastrado pelo dono | Acessar, visualizar ou alterar qualquer informação; receber avisos |

## 5. Regras de negócio

Cada regra numerada, atômica e testável.

**Acesso**

- RN01 — Todo acesso a objetos, contatos e empréstimos exige login. Sem login, o sistema mostra apenas as telas de entrar e de criar conta.
- RN02 — Para criar conta, são obrigatórios um e-mail válido, ainda não usado por outra conta, e uma senha com pelo menos 8 caracteres.
- RN03 — Cada conta vê e altera apenas os próprios objetos, contatos e empréstimos.
- RN04 — Se o e-mail ou a senha estiverem errados, o sistema exibe "E-mail ou senha incorretos", sem indicar qual dos dois está errado.

**Objetos**

- RN05 — Um objeto pode ter no máximo um empréstimo em aberto por vez.
- RN06 — O nome do objeto é obrigatório e tem de 1 a 100 caracteres, desconsiderando espaços nas pontas.
- RN07 — A categoria é obrigatória e deve ser uma das opções: CD, DVD/Blu-ray, Video game, Livro, Jogo de tabuleiro, Outros.
- RN08 — O estado de conservação é obrigatório e deve ser um dos valores: Novo, Ótimo, Bom, Regular, Danificado.
- RN09 — Um objeto com empréstimo em aberto não pode ser excluído.
- RN10 — O status exibido do objeto é calculado: "Emprestado" se houver empréstimo em aberto; "Extraviado" se o último empréstimo foi encerrado como não devolvido; "Disponível" nos demais casos. Para objeto emprestado, a lista de objetos e o detalhe do objeto mostram com quem ele está e a data limite, no formato "Emprestado — com Ana Souza até 17/11/2026". Se o empréstimo estiver atrasado, o texto termina com "(atrasado)".

**Contatos**

- RN11 — Um contato com empréstimo em aberto não pode ser excluído.
- RN12 — O nome do contato é obrigatório (1 a 100 caracteres). Telefone e e-mail são opcionais, mas, se informados, devem ter formato válido.

**Empréstimos**

- RN13 — Para registrar um empréstimo, são obrigatórios: objeto, contato, data do empréstimo e prazo em dias.
- RN14 — Só pode ser emprestado um objeto com status "Disponível" (consequência da RN05 e da RN41).
- RN15 — A data do empréstimo vem preenchida com a data de hoje (RN22) e não pode ser uma data futura.
- RN16 — A data do empréstimo não pode ser anterior à data de devolução do empréstimo mais recente do mesmo objeto. Pode ser igual a ela (o objeto voltou e saiu de novo no mesmo dia).
- RN17 — O prazo é um número inteiro de pelo menos 1 dia, sem limite máximo.
- RN18 — A data limite é calculada como data do empréstimo + prazo em dias. O dono não a edita diretamente.
- RN19 — O estado de conservação do objeto no momento da saída fica gravado no empréstimo e não muda se o objeto for editado depois.
- RN20 — Um contato com qualquer empréstimo atrasado não pode receber novo empréstimo até que todos os seus atrasos sejam encerrados.
- RN21 — O empréstimo tem um campo de observação opcional (até 500 caracteres), editável enquanto estiver em aberto, por exemplo para anotar "vou precisar de volta no dia 10".

**Prazo, vencimento e atraso**

- RN22 — Toda referência a "hoje" nas validações e nos cálculos (RN15, RN23, RN28, RN34, RN45, RN52) usa a data atual no horário de Brasília (America/Sao_Paulo), independentemente do fuso do computador.
- RN23 — A data limite e os dias faltantes são sempre exibidos juntos: na tela de novo empréstimo (assim que o prazo é preenchido), na lista de empréstimos e no detalhe. Os dias faltantes são data limite − hoje; quando forem 0, o texto é "vence hoje".
- RN24 — Todo vencimento acontece às 23:59 do dia, no horário de Brasília. O empréstimo continua em dia até o fim desse minuto e passa a atrasado às 00:00 do dia seguinte.
- RN25 — Dia útil é de segunda a sexta-feira, exceto feriados nacionais e os pontos facultativos marcados pelo dono como não úteis (RN57). Essas datas, inclusive as móveis (Carnaval, Sexta-feira Santa e Corpus Christi), são calculadas automaticamente pelo sistema para qualquer ano, sem cadastro manual.
- RN26 — Se a data limite não for dia útil, ou se o dia seguinte à data limite não for dia útil, o vencimento efetivo passa para as 23:59 do primeiro dia útil posterior à data limite. Nos demais casos, o vencimento efetivo é a própria data limite às 23:59.
- RN27 — Um empréstimo é considerado atrasado quando o momento atual, no horário de Brasília, é posterior ao vencimento efetivo e ele ainda não foi encerrado.
- RN28 — Durante a tolerância, o empréstimo exibe "Prazo venceu em DD/MM/AAAA; devolução sem atraso até DD/MM/AAAA". Quando atrasado, exibe "Atrasado há N dias", em que N = hoje − data do vencimento efetivo.

**Renovação**

- RN29 — Só um empréstimo ativo pode ser renovado. Empréstimos atrasados ou encerrados não podem. O bloqueio vale só para o empréstimo atrasado: os outros empréstimos ativos do mesmo contato podem ser renovados normalmente.
- RN30 — Na renovação, o dono informa N dias adicionais (N inteiro ≥ 1, sem limite máximo). A nova data limite é a data limite atual + N.
- RN31 — Cada renovação fica registrada no histórico do empréstimo com a data da renovação, os dias adicionados, a data limite anterior e a nova.

**Devolução**

- RN32 — Só um empréstimo em aberto pode ter devolução registrada.
- RN33 — Para registrar a devolução, são obrigatórios a data de devolução e o estado de conservação no retorno (mesma escala da RN08).
- RN34 — A data de devolução vem preenchida com a data de hoje (RN22), não pode ser futura e não pode ser anterior à data do empréstimo.
- RN35 — Ao registrar a devolução, o estado de conservação do objeto passa a ser o estado informado no retorno.
- RN36 — Se a data de devolução for posterior à data do vencimento efetivo, a devolução é marcada "com atraso" e os dias de atraso são data de devolução − data do vencimento efetivo. Os dias de atraso ficam gravados no momento do registro da devolução.
- RN37 — A devolução pode ser registrada antes da data limite. Se o dono marcar a opção "Solicitada pelo dono", a justificativa (5 a 500 caracteres) passa a ser obrigatória.

**Perda ou não devolução**

- RN38 — Só um empréstimo em aberto pode ser registrado como não devolvido.
- RN39 — Para registrar a não devolução, são obrigatórios a data em que o objeto deveria ter sido devolvido (vem preenchida com a data limite atual) e o motivo (texto de 5 a 500 caracteres).
- RN40 — A data em que o objeto deveria ter sido devolvido não pode ser anterior à data do empréstimo.
- RN41 — O registro de não devolução encerra o empréstimo e o objeto passa ao status "Extraviado", que não pode ser emprestado.

**Lista de empréstimos**

- RN42 — A situação de cada empréstimo, para fins de filtro, é uma das seguintes: Vigente (empréstimo ativo, inclusive em tolerância), Atrasado ou Encerrado (devolvido ou não devolvido).
- RN43 — A lista de empréstimos abre filtrada por Vigente e Atrasado, ordenada pela data limite da mais próxima para a mais distante. O dono pode marcar ou desmarcar cada situação, inclusive Encerrado.

**Painel da tela inicial**

- RN44 — A tela inicial exibe o painel com as seções "Vencem em breve" e "Atrasados", calculadas no momento em que a tela é aberta ou recarregada. Seção sem itens exibe "Nenhum empréstimo vence nos próximos dias" ou "Nenhum empréstimo atrasado".
- RN45 — Um empréstimo está com prazo próximo quando está ativo e a data de hoje é igual ou posterior à data limite − 2 dias, até o vencimento efetivo.
- RN46 — Cada empréstimo com prazo próximo aparece em "Vencem em breve" com objeto, contato, data limite e o texto "vence em 2 dias", "vence amanhã", "vence hoje" ou, na tolerância, o texto da RN28. Os itens são ordenados pela data limite.
- RN47 — O aviso de prazo próximo deixa de ser exibido quando o empréstimo é encerrado, quando passa a atrasado ou quando é renovado para uma data limite fora da janela da RN45.
- RN48 — Cada empréstimo atrasado aparece em "Atrasados" com objeto, contato e o texto "atrasado há N dias" (RN28), do mais atrasado para o menos atrasado.
- RN49 — O alerta de atraso de um empréstimo sai do painel apenas quando ele é encerrado pelo registro da devolução ou da não devolução.

**Histórico do objeto**

- RN50 — O histórico do objeto lista todos os empréstimos desse objeto, do mais recente para o mais antigo.
- RN51 — Cada item do histórico mostra: contato, data do empréstimo, data limite final, data de encerramento, situação, tempo emprestado, se houve atraso (e quantos dias), estado de conservação na saída e no retorno, renovações, observações e, se houver, a justificativa da devolução solicitada pelo dono ou o motivo da não devolução.
- RN52 — O tempo emprestado, em dias corridos, é a data de encerramento − data do empréstimo. Para empréstimo em aberto, usa-se a data de hoje. Para não devolvido, usa-se a data em que deveria ter sido devolvido.
- RN53 — "Houve atraso" é "Sim" quando o empréstimo foi devolvido com atraso (RN36) ou está atrasado (RN27); nos demais casos é "Não". Para não devolvido, o histórico mostra "Não devolvido" no lugar.
- RN54 — Ao excluir um contato sem empréstimo em aberto, os empréstimos encerrados dele continuam no histórico dos objetos, mas o contato aparece como "Contato removido".
- RN55 — Ao excluir um contato, os textos livres dos empréstimos dele (observação, justificativa da devolução solicitada pelo dono e motivo da não devolução) são apagados e passam a ser exibidos como "Texto removido junto com o contato". Datas, estados de conservação, situação, renovações e atrasos continuam no histórico.
- RN56 — Ao excluir um objeto, o histórico dele é excluído junto.

**Pontos facultativos**

- RN57 — Nas configurações da conta, o dono marca ou desmarca cada ponto facultativo como dia não útil: "Carnaval (segunda e terça)", "Quarta-feira de Cinzas" e "Corpus Christi". Feriados nacionais não podem ser desmarcados.
- RN58 — Toda conta nova começa com a sugestão do sistema: Carnaval e Corpus Christi marcados como não úteis; Quarta-feira de Cinzas desmarcada (útil).
- RN59 — Uma alteração nos pontos facultativos vale imediatamente para todos os empréstimos em aberto, cujo vencimento efetivo é recalculado. Devoluções já registradas mantêm os dias de atraso gravados (RN36).

**Exclusão de empréstimo**

- RN60 — O dono pode excluir qualquer empréstimo, em aberto ou encerrado, para corrigir um registro feito por engano. A exclusão exige confirmação ("Excluir o empréstimo de [objeto] para [contato]? Ele sai do histórico e não pode ser recuperado.") e remove também as renovações desse empréstimo.
- RN61 — Depois da exclusão, tudo o que depende de empréstimos é recalculado como se ele nunca tivesse existido: status do objeto (RN10), bloqueio do contato (RN20), última devolução usada na RN16, painel (RN44) e histórico do objeto (RN50).
- RN62 — A exclusão não altera o estado de conservação atual do objeto. Se a devolução excluída tiver mudado esse estado (RN35), o dono corrige editando o objeto.

## 6. Histórias de usuário

Datas dos exemplos no calendário de 2026: 02/11 (segunda, feriado de Finados), 06/11 (sexta), 15/11 (domingo, feriado), 17/11 (terça), 19/11 (quinta), 20/11 (sexta, feriado da Consciência Negra), 23/11 (segunda).

### HU01 — Acessar o sistema

Como dono, quero criar minha conta e entrar com e-mail e senha, para que só eu veja meus objetos e os dados dos meus contatos.
Regras relacionadas: RN01, RN02, RN03, RN04
Critérios de aceite:

- Cenário: criar conta
  Dado que estou na tela de criar conta
  Quando informo o e-mail "victor@exemplo.com" e uma senha de 10 caracteres
  Então a conta é criada e vejo a tela inicial vazia
- Cenário: e-mail já usado
  Dado que já existe uma conta com o e-mail "victor@exemplo.com"
  Quando tento criar outra conta com esse e-mail
  Então vejo a mensagem "Já existe uma conta com este e-mail" e nada é salvo
- Cenário: senha curta
  Dado que estou na tela de criar conta
  Quando informo uma senha de 6 caracteres
  Então vejo a mensagem "A senha deve ter pelo menos 8 caracteres" e nada é salvo
- Cenário: senha errada
  Dado que tenho uma conta
  Quando tento entrar com a senha errada
  Então vejo a mensagem "E-mail ou senha incorretos" e continuo na tela de entrar
- Cenário: sair
  Dado que estou logado
  Quando escolho "Sair"
  Então volto para a tela de entrar e não consigo ver meus dados sem entrar de novo
- Cenário: dados de outra conta
  Dado que outra pessoa tem conta no sistema com seus próprios objetos
  Quando entro com a minha conta
  Então vejo apenas os meus objetos, contatos e empréstimos

### HU02 — Cadastrar objeto

Como dono, quero cadastrar um objeto, para poder emprestá-lo depois.
Regras relacionadas: RN06, RN07, RN08, RN10
Critérios de aceite:

- Cenário: cadastro válido
  Dado que estou na tela de novo objeto
  Quando informo o nome "The Last of Us Part II", a categoria "Video game" e o estado "Ótimo"
  Então o objeto aparece na lista com status "Disponível"
- Cenário: nome vazio
  Dado que estou na tela de novo objeto
  Quando tento salvar sem nome
  Então vejo a mensagem "Informe o nome do objeto" e nada é salvo
- Cenário: estado de conservação não informado
  Dado que estou na tela de novo objeto
  Quando tento salvar sem escolher o estado de conservação
  Então vejo a mensagem "Informe o estado de conservação" e nada é salvo

### HU03 — Consultar objetos

Como dono, quero ver a lista dos meus objetos, para saber o que tenho e o que está emprestado.
Regras relacionadas: RN10
Critérios de aceite:

- Cenário: lista com status
  Dado que tenho o objeto "Dom Casmurro" emprestado, o objeto "Abbey Road" sem empréstimo e o objeto "FIFA 23" registrado como não devolvido
  Quando abro a lista de objetos
  Então vejo "Dom Casmurro" com status "Emprestado", "Abbey Road" com status "Disponível" e "FIFA 23" com status "Extraviado"
- Cenário: com quem está o objeto emprestado
  Dado que "Dom Casmurro" está emprestado a "Ana Souza" com data limite 17/11/2026
  Quando abro a lista de objetos
  Então vejo "Dom Casmurro" com "Emprestado — com Ana Souza até 17/11/2026"
- Cenário: objeto com empréstimo atrasado
  Dado que hoje é 19/11/2026 e "Dom Casmurro" está com "Ana Souza", com data limite terça 17/11/2026
  Quando abro a lista de objetos
  Então vejo "Emprestado — com Ana Souza até 17/11/2026 (atrasado)"
- Cenário: filtro por categoria
  Dado que tenho objetos das categorias "Livro" e "CD"
  Quando filtro pela categoria "Livro"
  Então vejo apenas os objetos da categoria "Livro"

### HU04 — Editar objeto

Como dono, quero editar os dados de um objeto, para mantê-los corretos, por exemplo quando o estado de conservação muda.
Regras relacionadas: RN06, RN07, RN08, RN19
Critérios de aceite:

- Cenário: alterar estado de conservação
  Dado que o objeto "Abbey Road" tem estado "Bom"
  Quando altero o estado para "Regular" e salvo
  Então o objeto passa a exibir estado "Regular"
- Cenário: edição não altera empréstimos já feitos
  Dado que "Dom Casmurro" foi emprestado com estado "Ótimo"
  Quando altero o estado do objeto para "Danificado"
  Então o empréstimo continua mostrando o estado de saída "Ótimo"

### HU05 — Excluir objeto

Como dono, quero excluir um objeto que não tenho mais, para manter o acervo atualizado.
Regras relacionadas: RN09, RN56
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

### HU06 — Cadastrar contato

Como dono, quero cadastrar meus amigos como contatos, para registrar para quem empresto.
Regras relacionadas: RN12
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

### HU07 — Editar e excluir contato

Como dono, quero editar ou excluir um contato, para manter minha lista correta.
Regras relacionadas: RN11, RN12, RN54, RN55
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
- Cenário: textos livres apagados na exclusão
  Dado que "Bruno Lima" não tem empréstimo em aberto e o empréstimo de "FIFA 23" com ele foi encerrado como não devolvido com o motivo "Bruno perdeu o jogo na mudança"
  Quando excluo "Bruno Lima" e confirmo
  Então o histórico de "FIFA 23" mostra o contato "Contato removido", a situação "Não devolvido" e o motivo "Texto removido junto com o contato"
  E as datas do empréstimo continuam visíveis
- Cenário: aviso na confirmação da exclusão
  Dado que "Bruno Lima" tem empréstimos encerrados
  Quando escolho excluir "Bruno Lima"
  Então a confirmação informa que observações, justificativas e motivos dos empréstimos dele serão apagados

### HU08 — Registrar empréstimo

Como dono, quero registrar que emprestei um objeto a um contato, informando o prazo em dias, para saber até quando ele deve ser devolvido.
Regras relacionadas: RN05, RN13, RN14, RN15, RN16, RN17, RN18, RN19, RN20, RN21, RN22, RN23
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
- Cenário: "hoje" no horário de Brasília
  Dado que o computador do dono está no fuso de Lisboa e lá já são 01:00 de 03/11/2026, mas em Brasília ainda são 22:00 de 02/11/2026
  Quando abro a tela de novo empréstimo
  Então a data do empréstimo vem preenchida com 02/11/2026
  E a data 03/11/2026 é recusada como futura
- Cenário: empréstimo retroativo já atrasado
  Dado que hoje é 02/11/2026
  Quando informo a data do empréstimo 10/10/2026 e o prazo de 10 dias
  Então o empréstimo é salvo com data limite 20/10/2026 (terça, seguida de dia útil)
  E já aparece como "Atrasado há 13 dias"
- Cenário: empréstimo retroativo sobreposto ao anterior
  Dado que "Dom Casmurro" foi emprestado a "Bruno Lima" de 01/09/2026 a 10/09/2026 e está "Disponível"
  Quando registro um empréstimo de "Dom Casmurro" para "Ana Souza" com data 05/09/2026
  Então vejo a mensagem "Este objeto só voltou em 10/09/2026. A data do empréstimo não pode ser anterior" e nada é salvo
- Cenário: empréstimo retroativo no mesmo dia da última devolução
  Dado que "Dom Casmurro" foi devolvido em 10/09/2026
  Quando registro um empréstimo de "Dom Casmurro" com data 10/09/2026
  Então o empréstimo é salvo

### HU09 — Renovar empréstimo

Como dono, quero renovar um empréstimo por mais alguns dias, para que o contato fique mais tempo com o objeto.
Regras relacionadas: RN29, RN30, RN31
Critérios de aceite:

- Cenário: renovação válida
  Dado que o empréstimo de "Dom Casmurro" tem data limite 17/11/2026
  Quando renovo por 10 dias
  Então a data limite passa a ser 27/11/2026
  E a renovação aparece no histórico do empréstimo
- Cenário: renovação na tolerância
  Dado que o empréstimo tem data limite sexta 06/11/2026 e hoje é sábado 07/11/2026
  Quando renovo por 7 dias
  Então a data limite passa a ser 13/11/2026
- Cenário: dias adicionais inválidos
  Dado que estou renovando um empréstimo ativo
  Quando informo 0 dias adicionais
  Então vejo a mensagem "Informe pelo menos 1 dia" e a data limite não muda
- Cenário: empréstimo atrasado
  Dado que o empréstimo de "FIFA 23" está atrasado
  Quando abro esse empréstimo
  Então a opção de renovar não está disponível e vejo "Empréstimos atrasados não podem ser renovados"
- Cenário: contato com outro empréstimo atrasado
  Dado que "Bruno Lima" tem o empréstimo de "FIFA 23" atrasado e o de "Abbey Road" ativo, com data limite 17/11/2026
  Quando renovo o empréstimo de "Abbey Road" por 10 dias
  Então a data limite de "Abbey Road" passa a ser 27/11/2026
  E o empréstimo de "FIFA 23" continua sem a opção de renovar
- Cenário: empréstimo encerrado
  Dado que o empréstimo de "Abbey Road" está "Devolvido"
  Quando abro esse empréstimo
  Então a opção de renovar não está disponível

### HU10 — Registrar devolução

Como dono, quero registrar a devolução de um objeto com a data e o estado em que ele voltou, para encerrar o empréstimo e saber se o objeto voltou bem.
Regras relacionadas: RN26, RN32, RN33, RN34, RN35, RN36, RN37
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

### HU11 — Registrar perda ou não devolução

Como dono, quero registrar que um objeto foi perdido ou não será devolvido, informando a data em que deveria ter voltado e o motivo, para encerrar o empréstimo e manter o registro do que aconteceu.
Regras relacionadas: RN38, RN39, RN40, RN41
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

### HU12 — Listar e filtrar empréstimos

Como dono, quero ver os meus empréstimos e filtrá-los por situação, para saber rapidamente o que está vigente, atrasado ou encerrado.
Regras relacionadas: RN23, RN27, RN28, RN42, RN43
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

### HU13 — Ver o que vence em breve

Como dono, quero ver na tela inicial os empréstimos que estão perto de vencer, para lembrar o contato antes que atrase.
Regras relacionadas: RN44, RN45, RN46, RN47
Critérios de aceite:

- Cenário: vence em 2 dias
  Dado que hoje é 15/11/2026 e o empréstimo de "Dom Casmurro" para "Ana Souza" tem data limite 17/11/2026
  Quando abro a tela inicial
  Então vejo em "Vencem em breve" o item "Dom Casmurro com Ana Souza vence em 2 dias (17/11/2026)"
- Cenário: vence hoje
  Dado que hoje é 17/11/2026 e a data limite é 17/11/2026
  Quando abro a tela inicial
  Então vejo o item com o texto "vence hoje"
- Cenário: durante a tolerância
  Dado que hoje é domingo 08/11/2026 e a data limite foi sexta 06/11/2026
  Quando abro a tela inicial
  Então vejo em "Vencem em breve" o texto "Prazo venceu em 06/11/2026; devolução sem atraso até 09/11/2026"
- Cenário: fora da janela
  Dado que hoje é 14/11/2026 e a data limite é 17/11/2026
  Quando abro a tela inicial
  Então esse empréstimo não aparece em "Vencem em breve"
- Cenário: item some após renovação
  Dado que hoje é 15/11/2026 e o empréstimo com data limite 17/11/2026 aparece em "Vencem em breve"
  Quando renovo o empréstimo por 10 dias e volto à tela inicial
  Então ele não aparece mais em "Vencem em breve"

### HU14 — Consultar histórico do objeto

Como dono, quero ver o histórico de empréstimos de um objeto, para saber com quem ele já esteve, por quanto tempo e se houve atraso.
Regras relacionadas: RN50, RN51, RN52, RN53
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

### HU15 — Ver os atrasados

Como dono, quero ver na tela inicial cada empréstimo atrasado e há quantos dias, para não esquecer de cobrar a devolução.
Regras relacionadas: RN26, RN27, RN28, RN44, RN48, RN49
Critérios de aceite:

- Cenário: primeiro dia de atraso
  Dado que o empréstimo de "Dom Casmurro" com "Ana Souza" tem data limite terça 17/11/2026 e não foi devolvido
  Quando abro a tela inicial em 18/11/2026
  Então vejo em "Atrasados" o item "Dom Casmurro com Ana Souza está atrasado há 1 dia"
- Cenário: contagem atualizada
  Dado que o empréstimo continua atrasado
  Quando abro a tela inicial em 19/11/2026
  Então o item mostra "atrasado há 2 dias"
- Cenário: tolerância não é atraso
  Dado que a data limite foi sexta 06/11/2026
  Quando abro a tela inicial no sábado 07/11/2026
  Então o empréstimo não aparece em "Atrasados"
- Cenário: item sai com a devolução
  Dado que o empréstimo está atrasado
  Quando registro a devolução e volto à tela inicial
  Então ele não aparece mais em "Atrasados"
- Cenário: nenhum atraso
  Dado que não tenho empréstimos atrasados
  Quando abro a tela inicial
  Então vejo em "Atrasados" a mensagem "Nenhum empréstimo atrasado"

### HU16 — Escolher os pontos facultativos

Como dono, quero decidir quais pontos facultativos contam como dias sem expediente, para que a tolerância siga o meu critério.
Regras relacionadas: RN25, RN26, RN36, RN57, RN58, RN59
Critérios de aceite:

- Cenário: sugestão inicial
  Dado que acabei de criar minha conta
  Quando abro as configurações de pontos facultativos
  Então vejo "Carnaval (segunda e terça)" e "Corpus Christi" marcados e "Quarta-feira de Cinzas" desmarcada
- Cenário: desmarcar Carnaval
  Dado que "Carnaval (segunda e terça)" está marcado e um empréstimo em aberto tem data limite sexta 13/02/2026
  Quando desmarco "Carnaval (segunda e terça)" e salvo
  Então o empréstimo passa a mostrar "devolução sem atraso até 16/02/2026" em vez de 18/02/2026
- Cenário: marcar Quarta-feira de Cinzas
  Dado que "Carnaval (segunda e terça)" está marcado e um empréstimo em aberto tem data limite sexta 13/02/2026
  Quando marco "Quarta-feira de Cinzas" e salvo
  Então o empréstimo passa a mostrar "devolução sem atraso até 19/02/2026"
- Cenário: devolução já registrada não muda
  Dado que um empréstimo com data limite sexta 13/02/2026 foi devolvido em 18/02/2026 sem atraso, com Carnaval marcado
  Quando desmarco "Carnaval (segunda e terça)"
  Então essa devolução continua sem atraso no histórico
- Cenário: feriado nacional não aparece
  Dado que estou nas configurações de pontos facultativos
  Quando vejo a lista
  Então feriados nacionais, como a Sexta-feira Santa, não aparecem para desmarcar

### HU17 — Excluir empréstimo registrado por engano

Como dono, quero excluir um empréstimo que registrei errado, para manter o controle do acervo fiel ao que aconteceu de fato.
Regras relacionadas: RN10, RN16, RN20, RN60, RN61, RN62
Critérios de aceite:

- Cenário: excluir empréstimo em aberto
  Dado que registrei "Dom Casmurro" para "Bruno Lima", mas quem pegou foi "Ana Souza"
  Quando excluo esse empréstimo e confirmo
  Então o empréstimo não aparece mais na lista nem no histórico de "Dom Casmurro"
  E "Dom Casmurro" volta a "Disponível", e posso registrá-lo para "Ana Souza"
- Cenário: desistir na confirmação
  Dado que escolhi excluir um empréstimo
  Quando cancelo a confirmação
  Então nada muda
- Cenário: devolução registrada por engano
  Dado que "Dom Casmurro" foi emprestado a "Ana Souza" em 02/11/2026 com prazo de 15 dias e registrei a devolução por engano, mas o livro continua com ela
  Quando excluo esse empréstimo e registro de novo o empréstimo para "Ana Souza" com data 02/11/2026 e prazo de 15 dias
  Então o empréstimo fica em aberto com data limite 17/11/2026
- Cenário: exclusão libera o contato
  Dado que o único empréstimo atrasado de "Bruno Lima" foi registrado por engano
  Quando excluo esse empréstimo e confirmo
  Então "Bruno Lima" volta a poder receber novos empréstimos
- Cenário: estado de conservação não volta sozinho
  Dado que uma devolução registrada por engano mudou o estado de "Abbey Road" de "Ótimo" para "Regular"
  Quando excluo esse empréstimo
  Então "Abbey Road" continua com estado "Regular" até eu editar o objeto

## 7. Estados do empréstimo

Os estados Ativo e Atrasado são calculados a partir do momento atual (horário de Brasília) e do vencimento efetivo. Devolvido e Não devolvido são estados finais gravados pelo dono.

| Estado atual | Evento | Próximo estado | Regra |
| --- | --- | --- | --- |
| (nenhum) | Registrar empréstimo com vencimento efetivo ainda não passado | Ativo | RN13–RN18, RN26 |
| (nenhum) | Registrar empréstimo retroativo com vencimento efetivo já passado | Atrasado | RN15, RN27 |
| (nenhum) | Registrar empréstimo para contato com atraso | Recusado | RN20 |
| (nenhum) | Registrar empréstimo retroativo com data anterior à última devolução do objeto | Recusado | RN16 |
| Ativo | Passam as 23:59 do vencimento efetivo sem encerramento | Atrasado | RN24, RN26, RN27 |
| Ativo | Renovar | Ativo (nova data limite) | RN29–RN31 |
| Atrasado | Renovar | Sem mudança (renovação recusada) | RN29 |
| Ativo | Registrar devolução | Devolvido | RN32–RN35 |
| Atrasado | Registrar devolução | Devolvido (com atraso) | RN32–RN36 |
| Ativo / Atrasado | Registrar perda ou não devolução | Não devolvido | RN38–RN41 |
| Ativo / Atrasado / Devolvido / Não devolvido | Dono exclui o empréstimo e confirma | Empréstimo removido | RN60, RN61 |
| Devolvido / Não devolvido | Qualquer evento, exceto a exclusão | Sem mudança (estado final) | RN29, RN32, RN38 |

Correspondência com o filtro da lista (RN42): Ativo → Vigente; Atrasado → Atrasado; Devolvido e Não devolvido → Encerrado.

Exemplos de vencimento efetivo (RN26):

| Data limite | Situação dos dias seguintes | Vencimento efetivo |
| --- | --- | --- |
| Terça 17/11/2026 | Quarta é dia útil | Terça 17/11/2026, 23:59 |
| Sexta 06/11/2026 | Sábado e domingo não são úteis | Segunda 09/11/2026, 23:59 |
| Quinta 19/11/2026 | Sexta 20/11 é feriado, depois fim de semana | Segunda 23/11/2026, 23:59 |
| Domingo 15/11/2026 | A própria data limite não é útil | Segunda 16/11/2026, 23:59 |
| Quinta 02/04/2026 | Sexta 03/04 é Sexta-feira Santa (feriado móvel), depois fim de semana | Segunda 06/04/2026, 23:59 |
| Sexta 13/02/2026 | Fim de semana, depois segunda 16/02 e terça 17/02 de Carnaval (marcados na sugestão) | Quarta 18/02/2026 (Cinzas), 23:59 |
| Quarta 03/06/2026 | Quinta 04/06 é Corpus Christi (marcado na sugestão); sexta é dia útil | Sexta 05/06/2026, 23:59 |

Os dois últimos exemplos usam a sugestão inicial da RN58; se o dono mudar a configuração, o vencimento efetivo muda junto (HU16).

## 8. Modelo de dados

**Conta**

| Atributo | Tipo | Obrigatório | Observação |
| --- | --- | --- | --- |
| id | identificador | sim | gerado pelo sistema |
| email | texto | sim | único, RN02 |
| senha | texto | sim | mínimo de 8 caracteres (RN02); guardada de forma cifrada, nunca em texto puro |
| carnaval_nao_util | booleano | sim | RN57; começa marcado (RN58) |
| cinzas_nao_util | booleano | sim | RN57; começa desmarcado (RN58) |
| corpus_christi_nao_util | booleano | sim | RN57; começa marcado (RN58) |
| criado_em | data e hora | sim | gerado pelo sistema |

**Objeto**

| Atributo | Tipo | Obrigatório | Observação |
| --- | --- | --- | --- |
| id | identificador | sim | gerado pelo sistema |
| conta_id | referência a Conta | sim | dono do objeto, RN03 |
| nome | texto (1–100) | sim | RN06 |
| categoria | enumeração | sim | RN07 |
| estado_conservacao | enumeração | sim | RN08; atualizado na devolução (RN35) |
| criado_em | data e hora | sim | gerado pelo sistema |
| atualizado_em | data e hora | sim | gerado pelo sistema |

O status (Disponível/Emprestado/Extraviado) não é gravado: é calculado pela RN10.

**Contato**

| Atributo | Tipo | Obrigatório | Observação |
| --- | --- | --- | --- |
| id | identificador | sim | gerado pelo sistema |
| conta_id | referência a Conta | sim | RN03 |
| nome | texto (1–100) | sim | RN12 |
| telefone | texto | não | formato validado se informado |
| email | texto | não | formato validado se informado |
| observacao | texto (até 500) | não | |

**Empréstimo**

| Atributo | Tipo | Obrigatório | Observação |
| --- | --- | --- | --- |
| id | identificador | sim | gerado pelo sistema |
| objeto_id | referência a Objeto | sim | |
| contato_id | referência a Contato | não | obrigatório no registro; fica vazio se o contato for excluído (RN54) |
| data_emprestimo | data | sim | RN15, RN16 |
| prazo_dias | inteiro (≥ 1) | sim | prazo original, RN17 |
| data_limite | data | sim | calculada (RN18), atualizada nas renovações (RN30) |
| estado_conservacao_saida | enumeração | sim | cópia do estado do objeto na saída, RN19 |
| observacao | texto (até 500) | não | editável enquanto em aberto, RN21; apagada se o contato for excluído (RN55) |
| encerramento | enumeração (Devolvido, Não devolvido) | não | vazio enquanto o empréstimo está em aberto |
| data_devolucao | data | não | obrigatória se encerramento = Devolvido (RN33, RN34) |
| estado_conservacao_retorno | enumeração | não | obrigatório se encerramento = Devolvido (RN33) |
| solicitada_pelo_dono | booleano | não | só na devolução, RN37 |
| justificativa_devolucao | texto (5–500) | não | obrigatória se solicitada_pelo_dono = sim (RN37); apagada se o contato for excluído (RN55) |
| data_prevista_devolucao | data | não | obrigatória se encerramento = Não devolvido (RN39, RN40) |
| motivo_nao_devolucao | texto (5–500) | não | obrigatório se encerramento = Não devolvido (RN39); apagado se o contato for excluído (RN55) |
| dias_atraso_devolucao | inteiro (≥ 0) | não | gravado no registro da devolução (RN36); não muda se a configuração de pontos facultativos mudar (RN59) |
| encerrado_em | data e hora | não | momento em que o dono registrou o encerramento |

Valores calculados, não gravados: vencimento efetivo (RN26), situação (se `encerramento` estiver preenchido, vale o seu valor; senão, Atrasado se o vencimento efetivo já passou; senão, Ativo), dias faltantes (RN23), dias de atraso de empréstimo em aberto (RN28), tempo emprestado (RN52) e prazo próximo (RN45).

**Renovação**

| Atributo | Tipo | Obrigatório | Observação |
| --- | --- | --- | --- |
| id | identificador | sim | |
| emprestimo_id | referência a Empréstimo | sim | |
| data_renovacao | data | sim | dia em que a renovação foi feita |
| dias_adicionais | inteiro (≥ 1) | sim | RN30 |
| data_limite_anterior | data | sim | RN31 |
| data_limite_nova | data | sim | RN31 |

**Não gravados:** os feriados nacionais e as datas dos pontos facultativos são calculados pelo sistema (RN25); só a escolha do dono fica gravada na Conta; o painel é calculado a partir dos empréstimos em aberto (RN44); o histórico é a consulta dos empréstimos de um objeto (RN50).

**Relacionamentos**

- Conta 1 — N Objeto; Conta 1 — N Contato
- Objeto 1 — N Empréstimo (no máximo 1 em aberto, RN05)
- Contato 1 — N Empréstimo
- Empréstimo 1 — N Renovação (as renovações são excluídas junto com o empréstimo, RN60)

## 9. Casos de borda e erros

| Situação | Comportamento esperado | Regra |
| --- | --- | --- |
| Acessar uma tela de dados sem login (ex.: pelo endereço da lista de objetos) | Redireciona para a tela de entrar | RN01 |
| Criar conta com e-mail já usado | "Já existe uma conta com este e-mail" | RN02 |
| Tentar emprestar objeto já emprestado ou extraviado | Objeto não aparece na lista de seleção; se o pedido chegar mesmo assim, é recusado | RN05, RN14, RN41 |
| Tentar emprestar para contato com empréstimo atrasado | Recusado com mensagem; o contato volta a poder receber empréstimos quando os atrasos forem encerrados | RN20 |
| Contato com empréstimo em tolerância (não atrasado) | Pode receber novo empréstimo normalmente | RN20, RN26 |
| Nome do objeto só com espaços | Tratado como vazio: "Informe o nome do objeto" | RN06 |
| Excluir objeto ou contato com empréstimo em aberto | Exclusão bloqueada com mensagem explicativa | RN09, RN11 |
| Prazo com decimais, negativo ou zero | "O prazo deve ser de pelo menos 1 dia" | RN17 |
| Prazo muito longo (ex.: 400 dias) | Aceito | RN17 |
| Data do empréstimo futura | "A data do empréstimo não pode ser futura" | RN15 |
| Empréstimo retroativo com vencimento efetivo já passado | Salvo e já exibido como "Atrasado" | RN15, RN27 |
| Empréstimo retroativo com data anterior à devolução do empréstimo anterior do mesmo objeto | Recusado: "Este objeto só voltou em DD/MM/AAAA. A data do empréstimo não pode ser anterior" | RN16 |
| Empréstimo com data igual à da última devolução do objeto | Permitido | RN16 |
| Computador do dono em outro fuso horário | Vencimento, atraso e "hoje" das validações calculados no horário de Brasília | RN22, RN24 |
| Entre 21:00 e 23:59 em Brasília com computador em fuso à frente (ex.: Lisboa já no dia seguinte) | A data de hoje continua sendo a de Brasília; o dia seguinte é recusado como futuro | RN22 |
| Vencimento às 23:59 | Às 23:59:30 ainda não é atraso; às 00:00 do dia seguinte já é | RN24 |
| Data limite na sexta | Sem atraso até segunda 23:59 | RN26 |
| Data limite na sexta e segunda é feriado | Sem atraso até terça 23:59 | RN26 |
| Data limite no sábado ou no domingo | Sem atraso até segunda 23:59 (ou o próximo dia útil) | RN26 |
| Data limite em dia útil seguido de dia útil | Atrasado a partir de 00:00 do dia seguinte | RN26, RN27 |
| Data limite véspera de feriado móvel (ex.: quinta 02/04/2026, antes da Sexta-feira Santa) | Sem atraso até segunda 06/04/2026 23:59 | RN25, RN26 |
| Data limite antes do Carnaval (ex.: sexta 13/02/2026), com a sugestão inicial | Sem atraso até quarta-feira de Cinzas 18/02/2026 23:59 | RN25, RN26, RN58 |
| Data limite na véspera de Corpus Christi (ex.: quarta 03/06/2026), com a sugestão inicial | Sem atraso até sexta 05/06/2026 23:59 | RN25, RN26, RN58 |
| Dono desmarca um ponto facultativo com empréstimo em tolerância por causa dele | O vencimento efetivo é recalculado na hora; o empréstimo pode passar a atrasado imediatamente | RN59 |
| Dono altera a configuração depois de devoluções já registradas | Os dias de atraso dessas devoluções não mudam | RN36, RN59 |
| Renovar durante a tolerância | Permitido; a nova data limite parte da data limite atual | RN29, RN30 |
| Renovar empréstimo atrasado | Recusado: "Empréstimos atrasados não podem ser renovados" | RN29 |
| Renovar empréstimo ativo de contato que tem outro empréstimo atrasado | Permitido; só o empréstimo atrasado fica sem renovação | RN29 |
| Renovar, devolver ou registrar perda de empréstimo encerrado | Opção indisponível | RN29, RN32, RN38 |
| Editar objeto emprestado | Permitido; o estado de saída gravado no empréstimo não muda | RN19 |
| Devolução no mesmo dia do empréstimo | Permitida; tempo emprestado = 0 dias | RN34, RN52 |
| Devolução exatamente no dia do vencimento efetivo | Não é atraso | RN36 |
| Devolução com data futura | "A data de devolução não pode ser futura" | RN34 |
| Devolução antes da data limite sem marcar "Solicitada pelo dono" | Aceita sem justificativa | RN37 |
| Objeto volta em estado pior que o da saída | Devolução aceita; o histórico mostra os dois estados | RN35, RN51 |
| Perda registrada antes da data limite (ex.: amigo avisou que perdeu) | Permitida; o dono pode trocar a data sugerida | RN39, RN40 |
| Motivo da não devolução com menos de 5 caracteres | "Informe o motivo (mínimo de 5 caracteres)" | RN39 |
| Objeto extraviado que reaparece | No MVP continua "Extraviado". Se o registro de não devolução foi um engano, o dono exclui o empréstimo e o objeto volta a "Disponível"; se o objeto foi mesmo perdido e reapareceu, o tratamento fica para a v1.1 (seção 2.3) | RN41, RN60 |
| Empréstimo atrasado | Sai de "Vencem em breve", entra em "Atrasados" e aparece no filtro "Atrasado" | RN47, RN48 |
| Vários empréstimos atrasados | Um item por empréstimo em "Atrasados", do mais atrasado para o menos | RN48 |
| Vários empréstimos com prazo próximo | Um item por empréstimo em "Vencem em breve", ordenados pela data limite | RN46 |
| Tela inicial aberta durante a virada do dia | O painel muda quando o dono recarrega a página ou volta à tela inicial | RN44 |
| Empréstimo atrasado registrado como não devolvido | Encerrado; sai de "Atrasados" | RN49 |
| Contato excluído com empréstimos encerrados | Histórico mantido, exibindo "Contato removido" | RN54 |
| Contato excluído com observação, justificativa ou motivo nos empréstimos dele | Os textos são apagados e exibidos como "Texto removido junto com o contato" | RN55 |
| Texto livre de um empréstimo de outro contato cita o nome do contato excluído | Não é alterado: o sistema não procura nomes em textos de outros contatos | RN55 |
| Excluir empréstimo com renovações | As renovações são excluídas junto | RN60 |
| Excluir a devolução mais recente de um objeto | A RN16 passa a usar a devolução anterior a ela; o empréstimo pode ser registrado de novo com a data original | RN16, RN61 |
| Excluir empréstimo atrasado | Sai de "Atrasados" e, se era o único atraso do contato, libera novos empréstimos para ele | RN20, RN61 |
| Objeto emprestado na lista de objetos | Mostra com quem está e a data limite; se atrasado, termina com "(atrasado)" | RN10 |
| Nenhum empréstimo para os filtros escolhidos | "Nenhum empréstimo encontrado para os filtros selecionados" | RN43 |

## 10. Requisitos não funcionais

- **Desempenho:** a lista de objetos, a de empréstimos, a tela inicial e o histórico de um objeto abrem em até 2 segundos com até 1.000 objetos e 5.000 empréstimos cadastrados na conta.
- **Usabilidade:** a data limite e os dias faltantes aparecem juntos e na hora, enquanto o dono digita o prazo (RN23). Datas no formato DD/MM/AAAA. O painel fica no topo da tela inicial, sem precisar abrir outra tela.
- **Acessibilidade:** campos com rótulos legíveis por leitor de tela, contraste mínimo WCAG 2.1 AA, e situações (Vigente/Atrasado/Encerrado) e itens do painel indicados por texto, não só por cor ou ícone.
- **Privacidade (LGPD):** os contatos são dados pessoais de terceiros. O sistema guarda apenas o necessário (nome e, se o dono quiser, telefone e e-mail), não compartilha esses dados, exige login para qualquer acesso (RN01) e separa os dados por conta (RN03). Ao excluir um contato, saem do sistema o cadastro, o vínculo com os empréstimos (RN54) e os textos livres dos empréstimos dele (RN55), porque podem conter informação pessoal ("Bruno perdeu o jogo na mudança"). Antes da exclusão, o sistema informa ao dono o que será apagado.
- **Segurança:** senhas guardadas de forma cifrada; acesso apenas por conexão segura (HTTPS).
- **Horário de referência:** todos os cálculos de vencimento, atraso e prazo próximo e o "hoje" das validações de data usam o horário de Brasília (America/Sao_Paulo), independentemente do fuso do computador (RN22, RN24).
- **Calendário de feriados:** os feriados nacionais e as datas dos pontos facultativos, fixos e móveis, são calculados pelo sistema para qualquer ano, sem tabela mantida à mão (RN25). Só a escolha do dono sobre os pontos facultativos é gravada (RN57).
- **Testabilidade:** o cálculo de vencimento efetivo, situação, dias faltantes, dias de atraso e prazo próximo depende apenas das datas do empréstimo e de um momento atual recebido como entrada, nunca lido diretamente do relógio do servidor. Os exemplos de vencimento efetivo da seção 7, os casos de borda da seção 9 e os cenários datados das histórias de usuário são os casos de teste de aceite dessas regras e devem ser escritos antes da implementação.
- **Plataforma:** sistema web, acessado pelo navegador e pensado para tela de computador (a partir de 1280 px de largura). Navegadores suportados: versões atuais de Chrome, Edge e Firefox. No navegador do celular as funções continuam acessíveis, mas sem layout dedicado. Os dados ficam em um servidor, para o dono acessar o mesmo acervo de qualquer computador.

## 11. Decisões e premissas

| ID | Decisão | Alternativas consideradas | Justificativa |
| --- | --- | --- | --- |
| D01 | Cada conta pertence a um único dono; contatos não acessam o sistema | Contatos com login próprio; acervo compartilhado | Mantém o escopo de um app pessoal, como o Anota Aí |
| D02 | Categorias vêm de uma lista fixa | Categorias livres criadas pelo dono | Evita duplicidade ("Livro" x "Livros") e facilita o filtro |
| D03 | Estado de conservação em escala fixa de 5 níveis | Texto livre | Permite comparar o estado na saída e na volta |
| D04 | Prazo em dias corridos; a data limite é calculada e exibida junto com os dias faltantes | Mostrar só um dos dois; dono escolhe a data limite | Definição do dono: os dois formatos ao mesmo tempo |
| D05 | Status do objeto e situação do empréstimo são calculados; só o encerramento é gravado | Gravar status e atualizar por rotina | Evita inconsistência, por exemplo um "Atrasado" que não foi atualizado |
| D06 | Estado de conservação da saída fica gravado no empréstimo | Ler sempre o estado atual do objeto | Preserva o histórico mesmo que o objeto seja editado depois |
| D07 | Data do empréstimo pode ser retroativa, mas não futura | Só a data de hoje; permitir datas futuras | Retroativa permite registrar empréstimos já feitos |
| D08 | Empréstimo atrasado não pode ser renovado; os outros empréstimos ativos do mesmo contato continuam renováveis | Permitir renovar atrasados; bloquear todas as renovações do contato | Definição do dono: o bloqueio é do empréstimo atrasado, não da pessoa |
| D09 | Na devolução, o estado de conservação do objeto é atualizado para o estado no retorno | Manter o estado do cadastro e só registrar o de retorno | O próximo empréstimo já sai com o estado real do objeto |
| D10 | Perda e não devolução são um único tipo de encerramento ("Não devolvido"), com motivo em texto livre | Dois tipos separados (Perda / Não devolução) | O pedido trata os dois casos juntos; o motivo diferencia |
| D11 | A data em que o objeto deveria ter sido devolvido vem preenchida com a data limite atual, mas pode ser alterada | Campo vazio; usar sempre a data limite | Evita digitação no caso comum e permite registrar perda antecipada |
| D12 | A lista de empréstimos mostra todas as situações, mas abre filtrada em Vigente e Atrasado | Lista só de ativos; abrir com todos | Atende ao pedido de "empréstimos ativos" e ao filtro "encerrado" |
| D13 | O aviso de prazo próximo começa 2 dias antes da data limite e segue durante a tolerância | Encerrar o aviso na data limite | Definição do dono (2 dias); durante a tolerância o dono ainda precisa ser lembrado |
| D14 | No MVP, avisos e alertas ficam só no painel da tela inicial, calculados ao abrir a tela | Notificação do navegador às 20:00 | O painel atende à proposta de valor sem agendador nem infraestrutura de notificação; a notificação fica para a v1.1 |
| D15 | Excluir contato mantém os empréstimos encerrados no histórico, sem nenhum dado do contato, inclusive os textos livres | Bloquear exclusão de contato com histórico; apagar os empréstimos; só avisar que os textos ficam | Preserva o histórico do objeto e atende à LGPD a baixo custo |
| D16 | Sem limite máximo de prazo nem de quantidade de renovações | Limitar prazo (ex.: 365 dias) ou número de renovações | Definição do dono ("a princípio não existe limite de prazo") |
| D17 | Quando o dono precisa do objeto antes do prazo, ele anota na observação e registra a devolução marcando "Solicitada pelo dono", com justificativa | Criar um fluxo de "pedido de devolução" com novo prazo | Definição do dono; não exige regra nova de prazo |
| D18 | Vencimento às 23:59 do horário de Brasília, para qualquer computador; o atraso começa às 00:00 do dia seguinte | Usar o fuso do computador; escrever 23:59:59 | Definição do dono; o mesmo empréstimo não fica atrasado em um computador e em dia no outro |
| D19 | Tolerância: se a data limite ou o dia seguinte não forem úteis, o vencimento vai para as 23:59 do primeiro dia útil posterior à data limite | Só adiar quando a própria data limite não é útil | Generaliza os exemplos do dono (sexta → segunda; feriado → próximo dia útil) |
| D20 | Feriados nacionais sempre contam como dia não útil; os pontos facultativos (Carnaval, Cinzas e Corpus Christi) são sugestões do sistema que o dono marca ou desmarca. Todas as datas são calculadas automaticamente | Fixar os pontos facultativos para todos; só feriados nacionais; incluir feriados estaduais e municipais | Definição do dono: o sistema sugere, mas quem decide é o dono. A sugestão inicial marca Carnaval e Corpus Christi e deixa Cinzas como útil, porque o ponto facultativo de Cinzas vale só até as 14h |
| D21 | Dias de atraso contam a partir do vencimento efetivo, não da data limite | Contar a partir da data limite | Se os dias de tolerância não são atraso, não devem entrar na contagem |
| D22 | O alerta de atraso sai do painel na devolução e também no registro de não devolução | Tirar só na devolução | O registro de não devolução também encerra o empréstimo |
| D23 | O bloqueio de novos empréstimos vale para o contato com qualquer empréstimo atrasado e termina quando todos forem encerrados | Bloqueio por prazo fixo após a devolução | Definição do dono: o bloqueio dura enquanto houver atraso |
| D24 | A data do empréstimo não pode ser anterior à última devolução do mesmo objeto | Verificar só empréstimos em aberto (RN05) | Impede períodos sobrepostos no histórico do objeto |
| D25 | O "hoje" das validações também usa o horário de Brasília | Usar a data do computador | Mesma razão da D18: as regras dão o mesmo resultado em qualquer computador |
| D26 | Sistema web, pensado para o computador; no celular funciona pelo navegador, sem layout dedicado | App nativo para celular; web responsiva com o mesmo peso | Definição do dono: uso principal no PC |
| D27 | Login com e-mail e senha desde o MVP, com dados separados por conta | Sistema sem login | Um sistema web publicado exporia dados pessoais de terceiros (LGPD) |
| D28 | O MVP não tem imagem do objeto | Imagem opcional desde o início | Imagem exige armazenamento, validação e redimensionamento e não muda nenhuma decisão do dono; fica para a v1.1 |
| D29 | O MVP não tem reserva; a renovação depende apenas de o empréstimo estar ativo | Especificar a reserva já no MVP | A reserva é a maior fonte de complexidade (conflitos de período, cancelamento, expiração); fica para a v2.0, quando a renovação passa a exigir período livre |
| D30 | No MVP, "Extraviado" é definitivo | Permitir que o objeto volte a "Disponível" | Evita regras de histórico para o reaparecimento; fica para a v1.1 |
| D31 | Mudar os pontos facultativos recalcula os empréstimos em aberto, mas não as devoluções já registradas, cujos dias de atraso ficam gravados | Recalcular também o histórico; aplicar a mudança só a empréstimos novos | O dono vê o efeito da escolha na hora nos empréstimos em andamento, e o histórico não muda depois de fechado. É a única exceção à D05 |
| D32 | O dono pode excluir qualquer empréstimo, em aberto ou encerrado, com confirmação; não há função de desfazer devolução | Excluir só empréstimos em aberto; função "desfazer devolução" que reabre o empréstimo | Definição do dono: ele tem controle total do acervo. Um único mecanismo corrige qualquer registro errado; uma devolução indevida é corrigida excluindo o empréstimo e registrando-o de novo com a data original |
| D33 | A lista de objetos mostra com quem está cada objeto emprestado e até quando | Mostrar só o status "Emprestado" | Atende diretamente à proposta de valor ("saber onde está cada objeto") sem abrir outra tela |

## 12. Questões em aberto

Nenhuma questão em aberto no momento.
