# SOLID - Teoria e Prática

## *SOLID* é um acrônimo de um conjunto de princípios da programação orientada a objeto.

- ### **SRP** : ***Single Responsability Principle (Princípio da Responsabilidade Única):***
    - "Uma classe deve ter um, e apenas um, motivo para ser modificada."
    ---
    ***Aqui temos um exemplo de uma violação do primeiro princípio.***
    ```python
    from datetime import datetime
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
    Na classe acima, temos a seguinte violação:
    - Uma classe não deve se adicionar ao banco, alguém deve fazer isso pra ela.

    No método **adicionar_cliente**, temos outras violações:
    - Um método que adiciona clientes, não precisa fazer validações, não precisa saber como fazer uma consulta SQL para adicionar o cliente ao banco de dados, e tanbém não precisa saber enviar emails.
    ---
    Também podemos observar que uma classe cliente, não deve, e nem pode ter um método que adiciona clientes, ela não deve ter a responsabilidade de saber como adicionar um cliente ao banco de dados.

    ---
    Agora se seguimos o Princípio de Responsabilidade Única, podemos dizer que a ***ÚNICA*** responsabilidade desta classe é, informar se o cliente é válido.

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
            return EmailServices.is_valid(self.email) and CpfServices(self.cpf)
    ```
    ---
    Ao aplicarmos o SRV, a classe passa a ter apenas a responsabilidade de validar o cliente.
    Agora também temos um método diferente e, esse método invoca outras duas classes que tem a responsabilidade de validar os dados do cliente.

    ---
    Para adicionar o cliente no banco de dados, teremos outra classe, chamada de ClienteRepository.
    
    Essa classe terá o método ***adicionar_cliente***, que irá fazer a consulta SQL para adicionar o cliente ao banco de dados.

    ***Obs: O método adicionar_cliente não precisa conhecer o banco de dados, ou seja, ele não precisa fazer a conexão com o banco, ele deve apenas inserir o cliente ao banco***

    ---
    E por último, temos a classe ClienteService, essa classe irá cadastrar o cliente.
    A classe terá o método ***adicionar_cliente***,
    esse método terá e responsabilidade de cadastrar o cliente

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
    Para solucionar esse problema, criaremos uma classe abstrata.

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
   No exemplo, temos a classe abstrata DebitoConta, e temos o método abstrato debitar. Para adicionarmos um novo tipo de conta, criamos uma classe DebitoContaCorrente que herda de DebitoConta , e sobrescrevemos o método debitar.

   Assim conseguimos adicionar um novo tipo de conta, sem ferir o OCP.
   
   ---
   ---
- ## **LSP** : ***Liskov Substitution Principle (Princípio da Substituição de Liskov):***
  - "Se q(x) é uma propriedade desmonstrável dos objetos x de tipo T. Então q(y) deve ser verdadeiro para objetos de Tipo S, onde S é um subtipo de T."
  - " Uma classe base deve poder ser substituída pela sua class derivada." 
 
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

 Acima temos a classe Retangulo tem um método que calcula a aréa, e temos a classe Quadrado, que herda de Retangulo por ter o mesmo calcúlo para a aréa, porém, ele sobrescreve o método **area**. Igualando altura e largura.
 
 Portanto, ao substituir a classe base Retangulo, o calcúlo para a área de um retangulo não seria mais correto.

 Pra deixar claro, vamos supor que passamos
 5 para altura e 10 para largura. Quadrado agora substitui a sua classe base.

O retorno desse calcúlo para o Retangulo deveria
ser 50, porém, ele irá retornar 25.


   
- **ISP** : ***Interface Principle***
- **DIP** : ***Principle***

