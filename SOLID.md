# SOLID - Teoria e Prática

## *SOLID* é um acrônimo de um conjunto de princípios da programação orientada a objeto.

- Nesse conjunto temos 5 princípios, sendo eles:
    - **SRP** : ***Single Responsability Principle (Princípio da Responsabilidade Única):***
      - Uma classe deve ter um, e apenas um, motivo para ser modificada.
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
       Temos a classe ClienteService, essa irá cadastrar o cliente.
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


    - **OCP** : ***Open/Closed Principle:***
    - **LSP** : ***Liskov Substitution Principle***
    - **ISP** : ***Interface Principle***
    - **DIP** : ***Principle***
