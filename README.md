Documentação autenticação aplicações de terceiros

## Solicitação de Tokens e cadastro de URL:

O primeiro passo é solicitar à DTI um token público para acesso ao login, um token privado para realizar requisições na API e cadastrar as urls para onde o usuário deverá ser direcionado após efetuar login.

## Efetuar login na API:

###### Botão de login:

Deve ser incorporada na aplicação do terceiro, um script que renderiza o botão de autenticação padrão da Minha Uno. Para instalar o botão de autenticação, adicione o script a seguir no `<head>` da sua página.

```
<script src="https://www.unochapeco.edu.br/minhauno/js/oauth/auth.min.js" type="text/javascript"></script>
```

Insira o código a seguir no lugar onde o botão "Entrar" deve aparecer:

```
<unochapeco />
<script>
    new UnoAuth({
        token: '',
        callback: ''
    });
</script>
```

###### Direcionamento para o login

Os parâmetros passados são um token público e uma url de redirecionamento do usuário após efetuar login. O token público será gerado e fornecido pela DTI e a url de redirecionamento deverá ser cadastrada junto a equipe da DTI.
O usuário só irá conseguir realizar login no sistema caso os parâmetros passados sejam válidos.

###### Redirecionamento pós login

Após efetuar login com sucesso, o usuário será redirecionado para a url que foi informada por parâmetro, e também será passado junto a url um código que deverá ser utilizado para acesso.
> Exemplo de retorno: https://www.meusistema.com.br/home?code=8bda15f3051fba9b42216782abe4c3ca.

## Acessar a API

Para saber a identificação do usuário à partir do código retornado, deverá ser consumido um serviço REST Unochapecó:

```
URL: 
  https://www.unochapeco.edu.br/minhauno/rpc/v1/api.getUser.json
Header: 
  Authorization: passar token privado
Parâmetros post x-www-form-urlencoded:
  code: código retornado na url de redirecionamento
```

Retornos jSON:

**Usuário inválido:**
```
{
  "status": false,
  "mensagem": "Texto da mensagem"
}
```
**Usuário válido:**
```
{
  "status": true,
  "data": {
    "nompes": "Nome usuário",
    "codpes": "Código do usuário",
    "e_mail": "email@usuario.com",
    "login": "login",
  }
}
```
