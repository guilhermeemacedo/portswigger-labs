# PortSwigger Lab – Privilege Escalation via Forged Cookie

## Contexto
Este laboratório da PortSwigger Web Security Academy demonstra uma falha de
escalada de privilégio vertical, onde um usuário comum consegue assumir o papel
de administrador por meio da manipulação de um cookie controlado pelo cliente.

O cenário representa uma aplicação web que utiliza cookies para diferenciar
usuários e administradores, mas não realiza validação adequada dessas
informações no backend.

---

## Vulnerabilidade
A vulnerabilidade ocorre quando a aplicação utiliza um cookie enviado pelo
cliente para determinar o nível de privilégio do usuário, sem validar se esse
valor foi gerado ou autorizado pelo servidor.

Nesse laboratório, o backend confia diretamente no valor do cookie
`admin=true/false` para conceder acesso a funcionalidades administrativas,
caracterizando uma falha de escalada de privilégio vertical e Broken Object
Level Authorization (BOLA).

---

## Exploração
Após realizar login com credenciais válidas de um usuário comum, a aplicação
define um cookie indicando o nível de privilégio do usuário, inicialmente como
`admin=false`.

Ao tentar acessar o endpoint `/admin`, o acesso é bloqueado. A requisição é
então interceptada utilizando o Burp Suite e enviada para o módulo Repeater.
Nesse ponto, o valor do cookie é modificado manualmente para `admin=true`.

A requisição alterada é reenviada e o backend aceita o valor sem validação,
permitindo o acesso ao painel administrativo. A partir desse painel, foi
possível executar a ação de exclusão do usuário Carlos, concluindo o laboratório.

---

## Impacto
Em sistemas reais, essa vulnerabilidade pode resultar em comprometimento total
da aplicação, permitindo que usuários comuns realizem ações administrativas,
como exclusão de contas, alteração de permissões e acesso a dados sensíveis.

No contexto do laboratório, um usuário sem privilégios conseguiu assumir o
papel de administrador e executar uma ação crítica, caracterizando um risco
grave de segurança.

---

## Mitigação
Para mitigar esse tipo de vulnerabilidade, a aplicação não deve confiar em
valores de cookies controlados pelo cliente para tomar decisões de autorização.

Boas práticas incluem:
- Validar privilégios no backend
- Associar papéis e permissões à sessão server-side
- Implementar controle de acesso baseado em funções (RBAC)
- Nunca confiar em dados fornecidos pelo cliente para definir nível de acesso

O backend deve sempre verificar se o usuário possui permissão adequada antes de
permitir ações administrativas.
