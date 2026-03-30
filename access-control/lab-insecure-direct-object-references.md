# PortSwigger Lab – Insecure Direct Object References (IDOR)

## Contexto
Este laboratório da PortSwigger Web Security Academy demonstra uma falha de
controle de acesso do tipo Insecure Direct Object Reference (IDOR), comum em
aplicações web e APIs modernas.

O objetivo do laboratório é compreender como a ausência de validação de
autorização no backend pode permitir o acesso indevido a dados de outros
usuários.

---

## Vulnerabilidade
A vulnerabilidade ocorre quando a aplicação utiliza referências diretas a
objetos internos, como arquivos ou identificadores numéricos, sem verificar
se o usuário autenticado possui permissão para acessá-los.

No laboratório, os registros de conversas do chat são armazenados no servidor
em arquivos identificados por nomes numéricos previsíveis.

---

## Exploração
Após realizar o login na aplicação, foi possível acessar o chat ao vivo e
gerar uma nova transcrição da conversa. Essa ação resultou na criação de um
arquivo de log no servidor, acessado diretamente por meio de uma requisição
HTTP.

A requisição foi interceptada utilizando o Burp Suite e enviada para o módulo
Repeater. Ao modificar o identificador do arquivo na URL (por exemplo, alterando
de `2.txt` para `1.txt`), o servidor retornou um arquivo pertencente a outro
usuário, sem qualquer verificação de autorização.

---

## Impacto
Em um cenário real, essa vulnerabilidade pode resultar em acesso não autorizado
a dados sensíveis, violando a privacidade dos usuários e legislações de
proteção de dados, como LGPD e GDPR.

No contexto do laboratório, o arquivo acessado continha informações sensíveis,
incluindo a senha de outro usuário, o que permitiu o comprometimento completo
da conta da vítima.

---

## Mitigação
Para mitigar esse tipo de vulnerabilidade, a aplicação deve implementar
controles rigorosos de autorização no backend, garantindo que o usuário
autenticado só tenha acesso aos objetos que lhe pertencem.

Outras boas práticas incluem:
- Não confiar em identificadores enviados pelo cliente
- Associar recursos ao usuário logado no servidor
- Evitar o uso de identificadores previsíveis
- Aplicar princípios de AppSec e recomendações do OWASP Top 10

A validação de acesso deve sempre ser realizada no lado do servidor.
