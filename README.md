# App Dashboard

Aplicação Laravel 13 + Inertia v3 + React 19 (TypeScript) + Tailwind v4, com autenticação via Laravel Fortify (cadastro, recuperação de senha, verificação de e-mail, 2FA e passkeys).

## Pré-requisitos

- PHP 8.3 ou superior (o CI usa 8.5), com as extensões `pdo_sqlite`, `mbstring`, `openssl` e `xml`
- Composer 2
- Node.js 22 e npm

## Rodando o projeto pela primeira vez

1. Clone o repositório e entre na pasta:

    ```bash
    git clone <url-do-repositorio> app-dashboard
    cd app-dashboard
    ```

2. Instale as dependências PHP e JS:

    ```bash
    composer install
    npm install
    ```

3. Crie o arquivo de ambiente a partir do exemplo:

    ```bash
    cp .env.example .env
    ```

    O `.env` não é versionado. Ajuste nele o que for necessário (nome da aplicação, URL, banco, e-mail). Os valores padrão já funcionam para desenvolvimento local com SQLite.

4. Gere a chave da aplicação (preenche a `APP_KEY` no `.env`):

    ```bash
    php artisan key:generate
    ```

    Sem essa chave, a aplicação não sobe e retorna o erro "No application encryption key has been specified".

5. Crie o arquivo do banco SQLite (ele não é versionado) e rode as migrations:

    ```bash
    touch database/database.sqlite
    php artisan migrate
    ```

6. Faça o build do frontend:

    ```bash
    npm run build
    ```

    > Atalho: `composer setup` executa os passos 2 a 6 de uma vez (o arquivo SQLite do passo 5 ainda precisa existir).

7. Suba o ambiente de desenvolvimento:

    ```bash
    composer dev
    ```

    Esse comando inicia, juntos, o servidor PHP (`php artisan serve`), o Vite com hot reload, o worker de filas e, quando disponível, o visualizador de logs (Pail).

8. Acesse **http://localhost:8000** e crie uma conta em "Register".

## Observações

- **E-mails**: em desenvolvimento, `MAIL_MAILER=log`. Os e-mails (verificação de conta, recuperação de senha) não são enviados de verdade e aparecem em `storage/logs/laravel.log`. O link de verificação está lá.
- **Dados de exemplo**: `php artisan db:seed` cria um usuário de teste: e-mail `test@example.com`, senha `password`.
- **Erro "Unable to locate file in Vite manifest"**: o Vite não está rodando. Use `composer dev` ou rode `npm run build`.

## Testes e qualidade de código

| Tarefa | Comando |
|---|---|
| Rodar os testes | `php artisan test --compact` |
| Rodar um arquivo de teste | `php artisan test --compact tests/Feature/DashboardTest.php` |
| Formatar o PHP | `composer lint` |
| Análise estática do PHP (Larastan) | `composer types:check` |
| Lint e formatação do JS/TS | `npm run check` (`npm run check:fix` para corrigir) |
| Checagem de tipos do TS | `npm run types:check` |
| Tudo o que o CI roda | `composer ci:check` |
