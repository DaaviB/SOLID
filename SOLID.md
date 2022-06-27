# SOLID - Teoria e Prática

## *SOLID* é um acrônimo de um conjunto de princípios da programação orientada a objeto.

- ### **SRP** : ***Single Responsability Principle (Princípio da Responsabilidade Única):***
    - "Uma classe deve ter um, e apenas um, motivo para ser modificada."
    ---
    ***Aqui temos um exemplo de uma violação do primeiro princípio.***
    ```python
    from datetime import 
    
    class Cliente:
        def __init__(self):
            self.cliente_id = 1
            self.nome = 'nome'
            self.email = 'email@email.com
            self.cpf = '33045673211'
            self.data_do_cadastro = datetime.now
        
        def adicionar_cliente(self):
            if '@' in self.email:
                return 'Cliente com email válido.'
            if len(self.cpf) == 11:
                return 'Cliente com cpf válido.'
            

            # Suponha que ouve uma consulta SQL -
            # para cadastrar o cliente no banco de dados.

            # E também criamos um script para que fosse - 
            # enviado um e-mail para o novo cliente cadastrado

            return 'Cliente cadastrado com sucesso.'
    ``` 
    ---
- Na classe acima, temos a seguinte violação:
    - Uma classe não deve se adicionar ao banco, alguém deve fazer isso pra ela.

- No método **adicionar_cliente**, temos outras violações:
    - Um método que adiciona clientes, não precisa fazer validações, não precisa saber como fazer uma consulta SQL para adicionar o cliente ao banco de dados, e também não precisa saber enviar emails.
    ---
- Também podemos observar que uma classe cliente, não deve, e nem pode ter um método que adiciona clientes, ela não deve ter a responsabilidade de saber como adicionar um cliente ao banco de dados.

    ---
- Agora, se seguimos o Princípio de Responsabilidade Única, podemos dizer que a ***ÚNICA*** responsabilidade desta classe é, informar se o cliente é válido.

    --- 
    Pondo em prática o ***SRV***:
    ```python
    class Cliente:
        def __init__(self):
            self.cliente_id = 1
            self.nome = 'nome'
            self.email = 'email@email.com
            self.cpf = '33045673211'
            self.data_do_cadastro = datetime.now
        def is_valid(self):
            return EmailServices.is_valid(self.email) and CpfServices.is_valid(self.cpf)
    ```
    ---
- Ao aplicarmos o ***SRV***, a classe passa a ter apenas a responsabilidade de validar o cliente.
    Agora também temos um método diferente e, esse método invoca outras duas classes que tem a responsabilidade de validar os dados do cliente.

    ---
- Para adicionar o cliente no banco de dados, teremos outra classe, chamada de **ClienteRepository**.
    
- Essa classe terá o método ***adicionar_cliente***, que irá fazer a consulta SQL para adicionar o cliente ao banco de dados.

    ---
- Por último, temos a classe **ClienteService**, essa classe irá cadastrar o cliente.
- A classe terá o método ***adicionar_cliente***,
    esse método terá à responsabilidade de cadastrar o cliente

    ```python
    class ClienteService():
    def __init__(self):
        cliente = Cliente()

    def adicionar_cliente(self):
        if not self.cliente.is_valid():
            return 'Dados Inválidos.'
        
        repo = ClienteRepository()
        repo.adicionar_cliente(cliente)
        
        EmailServices.enviar('email@email.com', cliente.email, 'Parabéns, você foi cadastrado.')


        return 'Cliente cadastrado com sucesso.'
        
    ```
    ---
    ---
- ### **OCP** : ***Open/Closed Principle (Princípio Aberto/Fechado):***
    - "Entidades de softwares (classes, módulos, funcões, etc.) devem estar abertas para extensões, mas fechadas para modificações."
    ---

    ***Violação do OCP:***
   ```python
   class DebitoConta:
        def debitar(valor, conta, tipo_conta):
            if tipo_conta == TipoConta.Corrente:
                ...
            if tipo_conta == TipoConta.Poupanca:
                ...
    ```
- TipoConta é um Enum.
- Supondo que temos um novo tipo de conta, teriamos que modificar a classe, adicionando outra validação para o novo tipo e támbem teriamos que modificr a classe **Enum** para adicionar um novo valor.

    ---
- Para solucionar esse problema, criaremos uma classe **abstrata**:

   ```python
   from abc import ABC, abstractmethod


    class DebitoConta(ABC):
        def __init(self):
            self.numero_transacao = None
            
        
        
        @abstractmethod
        def debitar(self, valor:float, conta:str):
            ...



    class DebitoContaCorrente(DebitoConta):
        def debitar(self, valor: float, conta: str):
            ... # Lógica que irá debitar em conta corrente.
            
            self.numero_transacao = 12455900
            
            return self.numero_transacao



    DebitoContaCorrente().debitar(1.2, 'Corrente')

   ```
   ---
- No exemplo, temos a classe abstrata **DebitoConta**, e temos o método abstrato **debitar**. Para adicionarmos um novo tipo de conta, criamos uma classe **DebitoContaCorrente** que herda de **DebitoConta** , e sobrescrevemos o método **debitar**.

- Assim conseguimos adicionar um novo tipo de conta, sem ferir o ***OCP***.
   
   ---
   ---
- ### **LSP** : ***Liskov Substitution Principle (Princípio da Substituição de Liskov):***
  - "Se q(x) é uma propriedade desmonstrável dos objetos x de tipo T. Então q(y) deve ser verdadeiro para objetos de Tipo S, onde S é um subtipo de T."
  - " Uma classe base deve poder ser substituída pela sua classe derivada." 
 
---
  ***Violação do LSP:***
 ```python
 class Retangulo    
    def area(self, altura, largura):
        return (altura * largura)

class Quadrado(Retangulo):
    def area(self, altura, largura):
        largura = altura
        return (largura * altura)

 ```
---
 - Acima temos a classe **Retangulo**, que tem um método que cálcula a área, e temos a classe **Quadrado**, que herda de **Retangulo** por ter o mesmo cálculo para a área, porém, ele sobrescreve o método **area**. Igualando altura e largura. 
 
 - Portanto, ao substituir a classe base **Retangulo**, o cálculo para a área de um **retangulo** não seria mais correto.

 - Para deixar claro, vamos supor que passamos
 5 para altura e 10 para largura. **Quadrado** agora substitui a sua classe base.

O retorno desse cálculo para o **Retangulo** deveria
ser 50, porém, ele irá retornar 25.

---
---
- ### **ISP** : ***Interface Segregation Principle (Princípio da Segregação de Interfaces***
  - "Clientes não devem ser forçados a depender de métodos que não usam." 

  ***Violação do ISP:***
 ```python
 class Cadastro:

    def validar_dados(self):
        # cpf, email, ...
        ...
    

    def salvar_no_banco(self):
        # Inserir os dados no banco.
        ...
    

    def enviar_email(self):
        # Envia o email
        ...
    

class CadastrarProdutos(Cadastro):
    def validar_dados(self):
        # Validando o produto
        ...
    

    def salvar_no_banco(self):
        # Insere o produto no banco.
        ...

    def enviar_email(self):
        # Opss... Produto não tem email...

 ```
---

- No exemplo acima, temos a classe **Cadastro** e nela temos 3 métodos: **validar_dados**, **salvar_no_banco**, **enviar_email**.
- Ao herdamos essa classe para cadastrarmos produtos, iremos nos deparar com um problema, temos um método que somos obrigados a implementar, porém, ele não terá nenhum sentindo, já que produtos não possuem email.

- Para solucionarmos esse problema, basta criarmos duas interface, **ICadastrarProduto** e **iCadastroCliente**.

```python
class ICadastrarProduto:
    def validar_dados(self):
        ...


    def salvar_no_banco(self):
        ...

class ICadastrarCliente:
    def validar_dados(self):
        ...
    
    def salvar_no_banco(self):
        ...
    
    def enviar_email(self):
        ...


class CadastrarProduto(ICadastrarProduto):
    def validar_dados(self):
        # Validando os dados
        ...

    def salvar_no_banco(self):
        # Inserindo no banco.
        ...


class CadastrarCliente(ICadastrarCliente):
    def validar_dados(self):
        # Validando os dados
        ...

    
    def salvar_no_banco(self):
        # Inserindo no banco.
        ...
    
    def enviar_email(self):
        # Enviando o email
        ...

```

- Agora temos duas interfaces para dois cadastros específicos, e com isso não ferimos o ***ISP***.

---
---
- ### **DIP** : ***Dependency Inversion Principle (Princípio da Inversão de Dependência)***
  - "Módulos de alto nível não devem depender de módulos de baíxo nível. Ambos devem depender de abstrações; abstrações não devem depender de detalhes. Detalhes devem depender de abstrações."
  - "Dependa de uma abstração e não de uma implementação."
---
- Relembrando o exemplo do ***SRP***.
    ```python
    class Cliente:
        def __init__(self):
            self.cliente_id = 1
            self.nome = 'nome'
            self.email = 'email@email.com
            self.cpf = '33045673211'
            self.data_do_cadastro = datetime.now
        def is_valid(self):
            return EmailServices.is_valid(self.email) and CpfServices.is_valid(self.cpf)
    ```


    ```python
    class ClienteService():
    def __init__(self):
        cliente = Cliente()

    def adicionar_cliente(self):
        if not cliente.is_valid():
            return 'Dados Inválidos.'
        
        repo = ClienteRepository()
        repo.adicionar_cliente(cliente)
        
        EmailServices.enviar('email@email.com', cliente.email, 'Parabéns, você foi cadastrado.')


        return 'Cliente cadastrado com sucesso.'
        
    ```

- No exemplo, temos a classe **Cliente**, que está dependendo de **EmailServices** e de **CpfServices**, também fazendo que o **SRP** seja parcialmente quebrado, já que caso alguma mudança feita em uma dessas duas classes afetasse seu **comportamento**, teriamos outro motivo para mudarmos ela.

- Esses mesmos problemas se repetem em **ClienteRepository**, **ClienteService**. Todas dependendo de implementações.   

- Para corrigirmos isso, criamos **interfaces**:
```python
from abc import ABC, abstractclassmethod, abstractmethod
from datetime import datetime
from typing import Type


# Interfaces
class IClientService(ABC):
    @abstractmethod
    def adicionar_cliente(self):
        ...


class IClientRepository(ABC):
    
    @abstractmethod
    def insert(self, dados):
        ...



class IEmailService(ABC):
    
    @abstractmethod
    def is_valid(self, email):
        ...
        
    @abstractmethod
    def enviar_email(self, mensagem, email):
        ...


class ICpfService(ABC):
    
    @abstractmethod    
    def is_valid(self, cpf):
        ...

# Implementações

class ClienteRepository(IClientRepository):
    
    def insert(self, cliente):
        # Insere ao banco que foi especificado
        print('Inserindo no Banco')
        ...


class EmailServices(IEmailService):
    def is_valid(self, email):
        # Validação do email
        print('Seu email é válido.')
        return True
    
    def enviar_email(self, mensagem, email):
        # Enviando email
        print('enviando email')
        print(mensagem)
        ...


class CpfServices(ICpfService):
    def is_valid(self, cpf):
        # Lógica de validação dos dados.
        print('Seu CPF é válido')
        return True
        ...


        
class Cliente():
    def __init__(self, email_service: Type[IEmailService], cpf_service: Type[ICpfService]):
        self.email_service = email_service
        self.cpf_service = cpf_service
        self.cliente_id = 1
        self.nome = 'nome'
        self.email = 'email@email.com'
        self.cpf = '33045673211'
        self.data_do_cadastro = datetime.now()
        
    def is_valid(self):
        return self.email_service.is_valid(self.email) and self.cpf_service.is_valid(self.cpf)

class ClienteService(IClientService):
    def __init__(self, cliente: Type[Cliente], email_service: Type[IEmailService],repository: Type[IClientRepository]):
        self.cliente = cliente
        self.repository = repository
        self.email_service = email_service
    
    
    def adicionar_cliente(self):
        print('adicionando...')
        if not self.cliente.is_valid():
            print('invalido')
            return 'Dados Inválidos.'
        
        self.repository.insert(self.cliente)
        self.email_service.enviar_email(
            mensagem='Minha mensagem de sucesso', 
            email=self.cliente.email, 
        )

        return 'Cliente cadastrado com sucesso.'

if __name__ = '__main__':
    cliente = Cliente(EmailServices(), CpfServices())
    cliente_service = ClienteService(cliente, EmailServices(), ClienteRepository())
    cliente_service.adicionar_cliente()

```
---
- Agora nossa classe **ClienteService** tem apenas um único motivo para mudar e depende somente de uma abstração, assim como as outras classes.


---
---


### Conclusão
***O uso dos princípios SOLID irá melhorar à escrita do seu código, manutenção e também ajudará outros devs que irão ler seu código e, entender como ele funciona, e assim poder implementar algo mais facilmente.***


***Nota: Tudo acima foi escrito durante meu aprendizado, pode haver erros ortográficos ou até mesmos nos códigos de exemplos. Caso note algum erro, entre em contato comigo para que eu possa corrigir.***

