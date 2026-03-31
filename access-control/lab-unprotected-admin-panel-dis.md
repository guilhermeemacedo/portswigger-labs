# PortSwigger Lab – Unprotected Admin Panel Discovered in JavaScript

## Contexto
Este laboratório da PortSwigger Web Security Academy demonstra uma falha de
Broken Object Level Authorization (BOLA) causada pela exposição de um painel
administrativo sem controles de acesso adequados.

O cenário representa uma aplicação web moderna onde o painel administrativo
não está protegido e sua localização é inadvertidamente exposta no código
JavaScript do front-end.

---

## Vulnerabilidade
A vulnerabilidade ocorre devido à ausência de validação de autenticação e
autorização no backend para uma funcionalidade administrativa sensível.

Embora o front-end utilize lógica JavaScript para ocultar visualmente o link
do painel administrativo, o endpoint continua acessível via URL direta. A
aplicação confia incorretamente na lógica do cliente para restringir acesso,
o que não constitui uma barreira de segurança.

Essa falha caracteriza um caso de BOLA, pois permite que usuários sem
privilégios administrativos executem ações de nível elevado.

---

## Exploração
Durante a análise da aplicação, os arquivos JavaScript foram inspecionados,
revelando uma lógica condicional que ocultava o link do painel administrativo
quando a variável `isAdmin` estava definida como falsa.

Mesmo com o link oculto, o código JavaScript expunha explicitamente o caminho
do painel administrativo (`/admin-e107b9`). A URL foi acessada manualmente no
navegador, resultando no carregamento do painel administrativo sem qualquer
verificação de autenticação ou autorização.

A partir desse painel, foi possível excluir o usuário Carlos, cumprindo o
objetivo do laboratório.

---

## Impacto
Em aplicações reais, essa vulnerabilidade pode levar ao acesso não autorizado
a funcionalidades administrativas, permitindo exclusão ou modificação de
usuários, alteração de permissões e comprometimento total do sistema.

No laboratório, um usuário sem privilégios administrativos conseguiu executar
uma ação crítica apenas conhecendo a URL do painel, caracterizando um risco
grave de segurança.

---

## Mitigação
Para mitigar esse tipo de vulnerabilidade, é essencial implementar controles
adequados de autenticação e autorização no backend para endpoints
administrativos.

Boas práticas incluem:
- Validar permissões no backend, independentemente da interface
- Implementar controle de acesso baseado em funções (RBAC)
- Nunca confiar em lógica ou variáveis do front-end para tomar decisões de
  segurança
- Tratar todo endpoint administrativo como sensível

O backend deve sempre verificar se o usuário possui o nível de privilégio
necessário antes de permitir qualquer ação administrativa.
