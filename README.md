# TrabalhoEmerson

# SIME - Sistema de Monitoramento de Erros Industriais

Sistema da **MetalTech** para registrar e acompanhar falhas nas máquinas da fábrica. Tem duas partes que usam o mesmo banco de dados:

- **Web (PHP + MySQL):** o operador registra falhas e o supervisor acompanha, resolve e exclui.
- **Mobile (Android, Xamarin.Forms):** registra e consulta ocorrências pelo celular, usando a `api.php` do sistema web.

## Perfis

- **Operador:** registra as falhas das máquinas.
- **Supervisor:** vê o dashboard, resolve ocorrências e exclui as pendentes.

## Web

### Arquivos

| Arquivo | Para que serve |
|---|---|
| `config.php` | Conexão com o banco, sessão e layout |
| `login.php` / `logout.php` | Entrar e sair |
| `operador.php` | Tela do operador |
| `dashboard.php` | Tela do supervisor |
| `resolver.php` | Marca uma ocorrência como resolvida |
| `deletar.php` | Exclui uma ocorrência pendente |
| `api.php` | API usada pelo app mobile |

### O que funciona

- Login por perfil (operador e supervisor).
- **Operador:** registrar ocorrência (data e hora automáticas), ver KPIs e os últimos 10 registros.
- **Supervisor:**
  - 5 KPIs: total, máquina mais crítica, erro mais comum, pendentes e resolvidas.
  - Gráficos de erros por dia (7 dias) e por mês (6 meses).
  - Tabela de ocorrências com filtro por status, que filtra assim que você escolhe.
  - Botão **Resolver** e botão **Deletar**. O Deletar só aparece nas pendentes.
  - A página recarrega sozinha a cada 10 segundos.

## Mobile

- Login fixo no app: `operador` / `123` e `supervisor` / `123`.
- Operador: registra ocorrências e vê seus KPIs e registros.
- Supervisor: vê KPIs, gráfico por mês e lista de ocorrências.
- A URL da API fica em `Db.cs`:
  - Emulador: `http://10.0.2.2/SIME/web/api.php?action=`
  - Celular: troque pelo IP do computador ou do servidor.

> Esta parte vem do README antigo do app. O código do mobile não foi analisado, então confirme rodando o app.

## API

| Ação | O que faz |
|---|---|
| `login` | Valida login e senha |
| `maquinas` | Lista as máquinas |
| `ocorrencias` | Lista as ocorrências |
| `registrar` | Cria uma ocorrência |

## O que ainda não funciona

- O mobile **não resolve nem deleta** ocorrências (a API não tem essas ações).
- Depois de resolver ou deletar no web, **nenhuma mensagem** de sucesso aparece.
- KPIs e gráficos do web **não mudam com o filtro** de status.
- O mobile ainda mostra Média Diária e Tendência Mensal, que foram **removidas do web**.
- O login do mobile é fixo, então o ID do operador pode não bater com o do banco.
- Não há cadastro de usuários nem de máquinas (só direto no banco).

## Segurança (melhorar antes de usar de verdade)

- Senhas guardadas em texto puro no banco.
- A `api.php` não exige login.
- Senha do banco escrita no `config.php`.
- O app usa HTTP, que o Android recente bloqueia por padrão.

## Banco de dados

Tabelas necessárias: `usuarios`, `maquinas` e `ocorrencias`. Na tabela `ocorrencias`, além dos dados da falha, precisam existir as colunas `status`, `data_resolucao` e `resolvido_por`.

## Como instalar

**Web**
1. Crie as tabelas no MySQL e cadastre um operador e um supervisor.
2. Coloque os dados do banco em `config.php`.
3. Copie os arquivos `.php` para o servidor e abra `index.php`.

**Mobile**
1. Confirme que `api.php?action=maquinas` abre no navegador.
2. Abra `AppMetalTech.sln` no Visual Studio e ajuste a `ApiUrl` em `Db.cs`.
3. Escolha `AppMetalTech.Android` como projeto inicial e execute.

## Autores

Protótipo desenvolvido para disciplina acadêmica.
