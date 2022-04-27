# Testando rotas com locust

## Introdução
Locust é uma ferramenta para enviar várias requisições simuiltaneas a uma ou mais rotas de modo a testar o modo como determinado endpoint lida com cargas pesadas.

## Configuração
Tenha Python 3 e PIP instalados no sistema [[1]](#1)

Digite no terminal `sudo pip3 install locust` e aperte enter.

## Usando o locust
O locust pode ser executado usando o comando `locust` no terminal. Ele vai procurar por um arquivo `locustfile.py` onde vai estar a configuração. Caso você dê o comando agora, um erro vai surgir. Além disso, podemos definir o arquivo a ser executado com `locust -f filename.py`.

### Criando um simples locustfile.py
Crie um arquivo chamado locustfile.py e coloque isso dentro dele:
```py
from locust import HttpUser, between, task

class WebsiteUser(HttpUser):
    wait_time = between(5, 15)
    
    @task
    def index(self):
        self.client.get("/")
```

Explicando o código:
- Na primeira linha importamos tudo o que vamos precisar
- Depois criamos uma classe WebsiteUser que herda a classe HttpUser. Isso significa que locust vai simular requests a partir de um navagador desktop.
- No wait_time definimos o tempo de espera entre cada requisição, nesse caso estamos usando between(5, 15), isso significa que vamos enviar requições em um intervalo que varia entre 5 e 15 segundos. Tambem podemos importar `constant` para usar um intervalo constante
- @task é um decorator para indicar que aquela função é uma task a ser executada pelo locust. Cada task é escolhida de maneira aleatória e todas tem a mesma chance de serem pegas, exceto se o parametro do decorator for passado, como: `@task(3)`, que significa que a task com esse decorator tem 3 vezes mais chance de ser escolhida.
- É criada uma função index, que vai testar nossa rota / (index)
- Chamados o método client.get para enviar requisições para a rota / (index)


## Conhecendo a dashboard
Quando você executa o locust, ele vai executar por padrão na seguinte url: http://localhost:8089. Para mudar a porta, basta usar --web-port __porta__.

Ao abrir a dashboard do locust pela primeira vez, você vai encontrar a seguinte tela: 

![print da tela](https://raw.githubusercontent.com/kamuridesu/simpledoc/main/locust/media/locust-initial-screen.png)

Onde:
- Number of users é a quantidade máxima de conexões que serão feitas com a rota.
- Spawn rate é a quantidade de usuários que serão adicionados aos usuários atuais até atingir o máximo, por exemplo:

```
Number of users: 1000
Spawn rate: 10
A cada segundo, 10 usuários abrem uma nova conexão e são adicionados aos usuários atuais
```

# End notes
<a id="1">[1]</a>
Caso não tenha o Python 3 ou PIP instalado, siga as instruções abaixo:
### Instalando Python 3 e PIP
#### Baseados em Debian (apt)
`$ sudo apt install python3-pip`
#### Baseados em Arch (pacman)
`$ sudo pacman -Sy python python-pip`
