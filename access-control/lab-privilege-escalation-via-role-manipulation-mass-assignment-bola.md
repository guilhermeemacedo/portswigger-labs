# PortSwigger Lab – Privilege Escalation via Role Manipulation (Mass Assignment → BOLA)

## Contexto
Este laboratório da PortSwigger Web Security Academy demonstra um encadeamento
de vulnerabilidades em uma aplicação web, onde uma falha de Mass Assignment
permite a escalada de privilégio vertical e resulta em uma violação de Broken
Object Level Authorization (BOLA).

O cenário representa uma API que permite atualização de dados de perfil de
usuários e utiliza o campo `roleId` para controlar níveis de acesso, incluindo
acesso ao painel administrativo.

---

## Vulnerabilidade
A vulnerabilidade raiz é **Mass Assignment**, que ocorre quando a API aceita
automaticamente campos enviados pelo cliente e os mapeia diretamente para o
objeto interno do backend sem validação de autorização por campo.

Nesse laboratório, o backend permitiu que o usuário alterasse o atributo
sensível `roleId`, elevando seus próprios privilégios. Essa falha levou a uma
**escalada de privilégio vertical** e culminou em uma vulnerabilidade de
**BOLA**, ao permitir acesso a funções administrativas.

---

## Exploração
Após efetuar login com credenciais válidas (`wiener:peter`), o usuário acessou
a página **My Account** e realizou a atualização do e-mail. A resposta da API
retornou dados internos do usuário, incluindo o campo `roleId`, inicialmente
definido como `1`.

A requisição de atualização foi interceptada e enviada ao Burp Suite Repeater.
Nesse momento, foi adicionado manualmente o campo `roleId` ao payload JSON,
alterando seu valor para `2`, correspondente a privilégios administrativos.

A API aceitou a modificação sem validação adicional. Em seguida, o usuário
acessou o endpoint `/admin`, obteve acesso ao painel administrativo e realizou
a exclusão do usuário Carlos, resolvendo o laboratório.

---

## Impacto
Em sistemas reais, esse tipo de vulnerabilidade pode resultar em escalada
completa de privilégios, acesso não autorizado a funcionalidades críticas,
exclusão ou modificação de usuários e comprometimento total da aplicação.

No laboratório, a combinação de Mass Assignment com falhas de autorização
permitiu que um usuário comum executasse ações administrativas, caracterizando
um risco crítico de segurança.

---

## Mitigação
Para mitigar esse tipo de vulnerabilidade, a aplicação deve:

- Impedir Mass Assignment por meio de listas de campos permitidos (allowlists)
- Separar DTOs de entidades internas de domínio
- Implementar validação de autorização por campo e por ação
- Centralizar a lógica de autorização no backend
- Utilizar modelos de controle de acesso como RBAC ou ABAC
- Nunca confiar em dados enviados pelo cliente para definir privilégios

Autenticação não implica autorização, e ambas devem ser tratadas de forma
independente e rigorosa no backend.
