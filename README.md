# Implantacao-Django-HEROKU

* Entre na sua pasta de projeto
`cd projetos`

* Instale, crie e ative seu virtualenv
```
sudo apt-get install python3-venv
virtualenv --python=python3.6 venv
. venv/bin/activate
```

* Instale, configure e inicie o repositorio de versões
```
sudo apt-get install git git-core
git config --global user.name "Seu Nome"
git config --global user.email seunome@email.com.br
git init
```

* Crie um arquivo chamado `.gitignore` com o seguinte conteúdo de exemplo:

```
.idea/
*.sqlite3
venv/
*.pyc
media/
.git/
__pycache__/
migrations/
.env
```
* Esconda informações importantes do `settings.py`
`pip3 install python-decouple`

Crie um arquivo `.env` no caminho raiz do projeto e insira as seguintes variáveis
```
SECRET_KEY=Your$eCretKeyHere (Obtenha esta chave secreta do settings.py)
DEBUG=False
```
Insira no `settings.py`
```
from decouple import config
SECRET_KEY = config('SECRET_KEY')
DEBUG = config('DEBUG', default=False, cast=bool)
```
* Configure o Banco de Dados
`pip3 install dj-database-url`

Insira no `settings.py` e remova as configurações de BD
```
from dj_database_url import parse as dburl
default_dburl = 'sqlite:///' + os.path.join(BASE_DIR, 'db.sqlite3')
DATABASES = { 'default': config('DATABASE_URL', default=default_dburl, cast=dburl), }
```

* Configure o Django para servir arquivos estáticos
`pip3 install dj-static`

Insira no wsgi.py e remova a linha `application = get_wsgi_application()`
```
from dj_static import Cling
application = Cling(get_wsgi_application())
```
Insira no `settings.py`
`STATIC_ROOT = os.path.join(BASE_DIR, 'staticfiles')`

* Crie requirements-dev.txt com somente a lista de bibliotecas necessárias para o desenvolvimento
`pip freeze > requirements-dev.txt`

* Crie um arquivo `requeriments.txt` referênciando o arquivo anterior com mais 02 bibliotecas
```
-r requirements-dev.txt
gunicorn
psycopg2==2.7.4
psycopg2-binary==2.7.4
```

* Crie um arquivo `Procfile` na raiz do projeto e adicione o seguinte código
```
web: gunicorn `altere_para_diretorio_que_contem_wsgi.py`.wsgi --log-file -
```
* Crie um arquivo `runtime.txt` para definir a versão do python que o heroku irá utilizar
`python-3.6.0`

* No seu terminal, instale o aplicativo HEROKU
Veja o comando de instalação para seu sistema operacional neste link http://bit.ly/2jCgJYW. Para o UBUNTU e derivados é:
`curl https://cli-assets.heroku.com/install-ubuntu.sh | sh`

* Crie url da aplicação e repositorio git no heroku para envio da aplicação
`heroku apps:create nome_aplicacao`

* Configure os hosts permitidos no `settings.py`
Inclua seu endereço na variável ALLOWED_HOSTS. Exemplo:
`ALLOWED_HOSTS = ['nome_aplicacao.herokuapp.com']`

* Instale o plugin de configuração
`heroku plugins:install heroku-config`

* Envie as variáveis de ambiente do arquivo `.env` para o HEROKU 
`heroku config:push`									

* Grave a versão do projeto no seu repositório local git
```
git add .
git commit -m 'Configuração para implantação'
```

* Envie a versão do projeto para o repositório remoto do heroku
`git push --force heroku master`

* Crie as tabelas do Banco de Dados com o comando remoto do App HEROKU
`heroku run python3 manage.py migrate`

* Crie o superusuário da sua aplicação
`heroku run python3 manage.py createsuperuser`

* Fim!

* Caso queira desabilitar o collectstatic para utilizar um outro repositório de arquivos. Ex:AMAZON
`heroku config:set DISABLE_COLLECTSTATIC=1`

* Caso queira habilitar a variável DEBUG para True
`heroku config:set DEBUG=True`
