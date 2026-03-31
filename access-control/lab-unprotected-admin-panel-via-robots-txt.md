# PortSwigger Lab – Unprotected Admin Panel via robots.txt

## Contexto
Este laboratório da PortSwigger Web Security Academy demonstra uma falha de
Broken Object Level Authorization (BOLA) causada pela exposição de um painel
administrativo sem qualquer controle de acesso.

O cenário simula uma aplicação web que possui funcionalidades administrativas
críticas acessíveis diretamente por meio de uma URL, assumindo que apenas
administradores conhecem o caminho correto.

---

## Vulnerabilidade
A vulnerabilidade ocorre pela ausência total de validação de autenticação e
autorização em um endpoint administrativo sensível.

A aplicação expõe um painel de administração acessível diretamente por URL,
permitindo que qualquer usuário — inclusive não autenticado — execute ações
críticas, como exclusão de usuários.

O problema não está no `robots.txt`, mas na confiança indevida de que esconder
uma URL é suficiente para protegê‑la.

---

## Exploração
Durante a análise da aplicação, o arquivo público `robots.txt` foi acessado.
Esse arquivo listava explicitamente o caminho `/administrator-panel` como uma
área que não deveria ser indexada por mecanismos de busca.

O caminho identificado foi acessado manualmente pelo navegador, sem qualquer
necessidade de autenticação. O painel administrativo foi carregado com todas
as funcionalidades disponíveis.

A partir desse painel, foi possível excluir o usuário Carlos, concluindo o
objetivo do laboratório sem a necessidade de credenciais privilegiadas.

---

## Impacto
Em um ambiente real, essa vulnerabilidade pode resultar em controle total da
aplicação, permitindo exclusão ou modificação de usuários, alteração de
permissões, vazamento de dados sensíveis e comprometimento completo do sistema.

No laboratório, um usuário não autenticado conseguiu executar uma ação
administrativa crítica, caracterizando um risco de segurança elevado.

---

## Mitigação
Para mitigar esse tipo de vulnerabilidade, é essencial proteger endpoints
administrativos com autenticação e autorização adequadas no backend.

Boas práticas incluem:
- Implementar controles de autorização baseados em função (RBAC)
- Validar permissões antes de executar qualquer ação sensível
- Nunca confiar em obscuridade de URLs como mecanismo de proteção
- Utilizar o `robots.txt` apenas para orientação de indexação, não para segurança

Funcionalidades administrativas devem sempre validar quem está realizando a
requisição antes de permitir qualquer operação.
