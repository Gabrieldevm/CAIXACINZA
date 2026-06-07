# Validação de API: Testes de Caixa Cinza com Supabase

## 1. Escopo do Projeto
Este repositório contém a documentação e execução de testes funcionais em uma API Rest. Utilizando as premissas de Teste de Caixa Cinza, o objetivo foi provisionar um serviço de autenticação na nuvem (Supabase) e validar as respostas do servidor (HTTP Status Codes, Body JSON e Headers) utilizando o Postman.

## 2. Estruturação do Backend (Supabase)
Para simular um ambiente de produção sem a necessidade de programar a infraestrutura, um projeto `qa-gabriel-auth` foi inicializado na plataforma Supabase.
* O provedor de e-mail/senha foi ativado.
* Foi inserido um usuário de testes diretamente no banco de dados (`gabriel.qa@facens.br`).
* As chaves públicas (`API Key`) e a rota raiz foram capturadas para uso no cliente de testes.

### Imagens da Configuração:
![Dashboard do Projeto](./img/telaprincipalsupabase.png)
![Usuário Inserido](./img/usersupabase.png)
![Configuração da API](./img/apisupabase.png)

## 3. Preparação do Cliente de Testes (Postman)
No Postman, as boas práticas de testes foram seguidas isolando os dados de conexão através de Variáveis de Ambiente (*Environment*). As seguintes chaves foram cadastradas:
* `base_url`: URL do Supabase.
* `api_key`: Chave de acesso da API.
* `email` e `password`: Massa de dados estática para o teste positivo.

### Evidência do Postman:
![Variáveis de Ambiente](./img/Configuracao-Requisicao.png)

## 4. Estrutura da Requisição POST
O disparo de teste foi configurado para a rota padrão de geração de tokens de sessão do Supabase.

* **Endpoint Alvo:** `/auth/v1/token?grant_type=password`
* **Método:** HTTP POST

**Cabeçalhos (Headers):**

| Header | Valor |
| :--- | :--- |
| `apikey` | [Chave Injetada via Variável] |
| `Content-Type` | `application/json` |

**Corpo da Requisição (Payload JSON):**

```json
{
  "email": "gabriel.qa@facens.br",
  "password": "SenhaForte#2026"
}
```
5. Bateria de Testes (Caixa Cinza)
Foram modelados cinco casos de teste para verificar a resiliência da API contra inputs indevidos.
<img width="1625" height="762" alt="Evidencia-01-Login-Valido" src="https://github.com/user-attachments/assets/6c5ce033-1caa-4144-909d-b2a82ab62e98" />
<img width="1635" height="758" alt="Evidencia-02-Senha-Incorreta" src="https://github.com/user-attachments/assets/6c5d035f-530d-46b3-be44-6e20e50f4e8a" />
<img width="1655" height="768" alt="Evidencia-03-Nao-Cadastrado" src="https://github.com/user-attachments/assets/983e2ffa-8c47-4d44-a24e-51c24fa3ab19" />
<img width="1637" height="768" alt="Evidencia-04-Campos-Vazios" src="https://github.com/user-attachments/assets/d358df2a-137b-4715-aab2-c778747b9c03" />
<img width="1651" height="771" alt="Evidencia-05-Formato-Invalido" src="https://github.com/user-attachments/assets/3c29444f-795a-4966-85d1-6b2f7589d942" />


6. Documentação (Planilha)
A matriz de rastreabilidade de testes foi documentada via planilha, garantindo que o histórico de validação do QA seja mantido para futuras regressões no código.
<img width="1221" height="365" alt="PLANILHACAIXACINZA" src="https://github.com/user-attachments/assets/bec1b217-efd3-44b3-8f51-c23634424c53" />

7. Análise Final e Conclusão
A bateria de testes demonstrou que o backend gerado pelo Supabase é altamente seguro e lida corretamente com as validações de input. Todos os cenários de erro foram barrados na porta (Status 400), e nenhuma informação sensível do banco de dados vazou nos responses. O token JWT foi gerado apenas sob condições ideais. A execução comprovou o valor do Teste de Caixa Cinza: por termos um conhecimento prévio da estrutura do JSON e dos Headers, conseguimos elaborar testes cirúrgicos que cobrem todas as vulnerabilidades comuns de uma rota de login.

Documentação feita por: Gabriel
