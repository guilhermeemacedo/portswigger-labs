# PortSwigger Lab – OAuth Implicit Flow Account Takeover

## Contexto
Este laboratório da PortSwigger Web Security Academy demonstra uma falha de
segurança associada ao uso incorreto do OAuth Implicit Flow, evidenciando por
que esse fluxo é considerado inseguro e desencorajado nas implementações
modernas de autenticação.

O objetivo do laboratório é mostrar como falhas de validação no backend podem
permitir o acesso indevido a contas de outros usuários, mesmo sem o
conhecimento de suas senhas.

---

## Vulnerabilidade
A vulnerabilidade ocorre devido à combinação de dois fatores críticos:
o uso do OAuth Implicit Flow e a confiança excessiva em parâmetros controlados
pelo cliente, como o e-mail do usuário.

Neste fluxo, a aplicação utiliza o token como prova direta de autenticação e
associa a sessão ao usuário com base em informações recebidas do navegador,
sem verificar no servidor de autorização se o token realmente pertence ao
usuário indicado.

Essa falha representa um problema grave de autenticação e autorização.

---

## Exploração
Durante o laboratório, foi iniciado o fluxo de autenticação OAuth utilizando o
Implicit Flow. O usuário realizou login com credenciais válidas fornecidas pelo
laboratório, recebendo um token de acesso legítimo.

A requisição responsável por concluir a autenticação foi interceptada utilizando
o Burp Suite. Nessa requisição, o parâmetro de e-mail foi alterado manualmente
para o endereço de outro usuário (Carlos), mantendo o token válido obtido no
fluxo OAuth.

Como a aplicação não validou a correspondência entre o token e o usuário no
backend, a sessão foi associada indevidamente à conta de Carlos, permitindo o
acesso completo à sua conta sem conhecimento da senha.

---

## Impacto
Em aplicações reais, essa vulnerabilidade pode causar account takeover,
comprometimento de APIs, acesso não autorizado a dados sensíveis e falhas
críticas em sistemas de identidade e acesso (IAM).

No contexto do laboratório, o impacto foi a obtenção de acesso à conta de outro
usuário apenas pela manipulação de parâmetros no fluxo de autenticação, o que
caracteriza um risco crítico de segurança.

---

## Mitigação
Para mitigar esse tipo de vulnerabilidade, recomenda-se evitar completamente o
uso do OAuth Implicit Flow e adotar o Authorization Code Flow com PKCE, que
realiza a troca dos tokens diretamente no backend.

Outras boas práticas incluem:
- Validar tokens no servidor de autorização
- Não confiar em parâmetros enviados pelo navegador
- Associar tokens a usuários no backend
- Implementar controles rigorosos de autenticação e autorização

O cliente nunca deve decidir a identidade do usuário autenticado.
