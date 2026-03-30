# PortSwigger Lab – User ID controlled by request parameter with unpredictable user IDs

## Contexto
Este laboratório da PortSwigger Web Security Academy demonstra uma falha de
controle de acesso do tipo Broken Object Level Authorization (BOLA), também
conhecida como Insecure Direct Object Reference (IDOR), em um cenário onde os
identificadores de usuário são imprevisíveis.

O objetivo do laboratório é mostrar que, mesmo com IDs gerados de forma segura,
a ausência de validações de autorização no backend pode permitir acesso indevido
a dados de outros usuários.

---

## Vulnerabilidade
A vulnerabilidade ocorre quando a aplicação utiliza identificadores diretos
para acessar recursos sensíveis sem validar se o usuário autenticado possui
permissão para acessá-los.

Embora os IDs utilizados sejam imprevisíveis, eles são expostos em diferentes
partes da aplicação, como perfis públicos e requisições da API, permitindo que
um atacante reutilize esses identificadores para acessar objetos de outros
usuários.

---

## Exploração
Após realizar o login com credenciais fornecidas pelo laboratório, foi possível
navegar pela aplicação e acessar postagens públicas. Ao clicar no perfil do
usuário Carlos, a aplicação expôs um identificador exclusivo associado a esse
usuário em uma requisição HTTP.

Ao acessar a funcionalidade "My Account", a aplicação realizou automaticamente
uma requisição de API utilizando o identificador do usuário autenticado para
retornar dados sensíveis, como a chave de API.

Essa requisição foi interceptada com o Burp Suite e enviada para o módulo
Repeater. Ao substituir o identificador do usuário autenticado pelo
identificador previamente obtido do perfil de Carlos, a API retornou
indevidamente os dados de Carlos, incluindo sua chave de API.

---

## Impacto
Em um cenário real, essa vulnerabilidade pode resultar em vazamento de dados
sensíveis, comprometimento de APIs, escalada de privilégios e violação de
legislações de proteção de dados, como LGPD e GDPR.

No laboratório, o impacto foi a exposição da chave de API de outro usuário,
permitindo o acesso não autorizado a recursos protegidos e o comprometimento
completo da conta da vítima.

---

## Mitigação
Para mitigar esse tipo de vulnerabilidade, a aplicação deve implementar
verificações rigorosas de autorização em nível de objeto no backend,
garantindo que o usuário autenticado só consiga acessar recursos que lhe
pertençam.

Boas práticas recomendadas incluem:
- Validar autorização antes de acessar qualquer recurso sensível
- Não confiar em IDs enviados pelo cliente
- Associar objetos ao usuário no lado do servidor
- Aplicar princípios de AppSec e recomendações do OWASP Top 10

IDs imprevisíveis dificultam ataques por força bruta, mas não substituem a
necessidade de controles adequados de autorização.
