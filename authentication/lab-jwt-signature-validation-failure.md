# PortSwigger Lab – JWT Signature Validation Failure

## Contexto
Este laboratório da PortSwigger Web Security Academy demonstra uma falha crítica
na validação de assinatura de tokens JWT, evidenciando como a falta de
verificação adequada pode permitir acesso administrativo não autorizado.

O objetivo do laboratório é mostrar os riscos de confiar em tokens JWT sem
validar sua autenticidade no backend, cenário comum em APIs e aplicações
modernas.

---

## Vulnerabilidade
A vulnerabilidade ocorre quando a aplicação utiliza tokens JWT como prova de
identidade, mas não valida corretamente sua assinatura antes de autorizar ações
sensíveis.

Nesse cenário, a aplicação confia apenas nos dados presentes no payload do
token, aceitando tokens manipulados ou forjados, desde que possuam uma
estrutura válida.

Essa falha compromete diretamente a integridade do mecanismo de autenticação.

---

## Exploração
Durante o laboratório, foi realizado login com credenciais válidas, permitindo
a obtenção de um token JWT armazenado no cookie de sessão.

O token foi interceptado utilizando o Burp Suite e copiado para a ferramenta
jwt.io, onde seu payload pôde ser visualizado e modificado. A alteração teve
como objetivo elevar privilégios, assumindo uma identidade administrativa.

O token modificado foi então inserido na requisição destinada ao endpoint
`/admin`. Como a aplicação não validou a assinatura do JWT, o token forjado foi
aceito, permitindo acesso ao painel administrativo.

A partir desse acesso, foi possível excluir o usuário Carlos, confirmando a
exploração da vulnerabilidade.

---

## Impacto
Em ambientes reais, essa falha pode resultar em comprometimento total da
aplicação, permitindo account takeover, acesso administrativo, modificação ou
exclusão de dados e vazamento de informações sensíveis.

No laboratório, o impacto foi a obtenção de privilégios administrativos e a
exclusão de um usuário, caracterizando um risco crítico de segurança.

---

## Mitigação
Para mitigar esse tipo de vulnerabilidade, é essencial implementar validação
rigorosa da assinatura dos tokens JWT no backend.

Boas práticas incluem:
- Validar a assinatura do JWT com a chave correta
- Rejeitar tokens com algoritmos inseguros ou não esperados
- Não confiar apenas no payload do token
- Associar identidade e privilégios ao backend, não ao cliente

A autenticidade do token deve sempre ser validada antes de qualquer decisão de
autorização.
