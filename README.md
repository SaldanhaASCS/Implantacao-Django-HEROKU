# Implantacao-Django-HEROKU

# ENTRE NA SUA PASTA DE PROJETO
* cd projetos

# INSTALE, CRIE E ATIVE SEU VIRTUALENV
* sudo apt-get install python3-venv
* virtualenv --python=python3.6 venv
* . venv/bin/activate

# INSTALE, CONFIGURE E INICIE O REPOSITORIO DE VERSÕES GIT
* sudo apt-get install git git-core
* git config --global user.name "Seu Nome"
* git config --global user.email seunome@email.com.br
* git init

# CRIE UM ARQUIVO CHAMADO `.gitignore` COM O SEGUINTE CONTEÚDO DE EXEMPLO:
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
# ESCONDA INFORMAÇÕES IMPORTANTES DO `settings.py`
* pip3 install python-decouple

Crie um arquivo `.env` no caminho raiz do projeto e insira as seguintes variáveis
- SECRET_KEY=Your$eCretKeyHere (Obtenha esta chave secreta do settings.py)
- DEBUG=False

Insira no settings.py
- from decouple import config
- SECRET_KEY = config('SECRET_KEY')
- DEBUG = config('DEBUG', default=False, cast=bool)

# CONFIGURE O BANCO DE DADOS
* pip3 install dj-database-url

Insira no `settings.py` e remova as configurações de BD
- from dj_database_url import parse as dburl
- default_dburl = 'sqlite:///' + os.path.join(BASE_DIR, 'db.sqlite3')
- DATABASES = { 'default': config('DATABASE_URL', default=default_dburl, cast=dburl), }

# CONFIGURE DJANGO PARA SERVIR ARQUIVOS ESTÁTICOS
* pip3 install dj-static

Insira no wsgi.py e remova a linha `application = get_wsgi_application()`
- from dj_static import Cling
- application = Cling(get_wsgi_application())

Insira no `settings.py`
- STATIC_ROOT = os.path.join(BASE_DIR, 'staticfiles')

# CRIE REQUIREMENTS-DEV.TXT COM A LISTA DE BIBLIOTECAS NECESSÁRIAS PARA O DESENVOLVIMENTO
* pip freeze > requirements-dev.txt

# CRIE UM ARQUIVO `requeriments.txt` REFERÊNCIANDO O ARQUIVO ANTERIOR COM MAIS 02 BIBLIOTECAS QUE SERÃO UTILIZADA PELO HEROKU
- -r requirements-dev.txt
- gunicorn
- psycopg2==2.7.4
- psycopg2-binary==2.7.4

# CRIE UM ARQUIVO NA RAIZ DO PROJETO `Procfile` E ADICIONE O SEGUINTE CÓDIGO
web: gunicorn `altere_para_diretorio_que_contem_wsgi.py`.wsgi --log-file -

# CRIE UM ARQUIVO `runtime.txt` PARA DEFINIR A VERSÃO DO PYTHON QUE O HEROKU IRÁ UTILIZAR
- python-3.6.0

# NO SEU TERMINAL, INSTALE O APLICATIVO HEROKU
Veja o comando de instalação para seu sistema operacional neste link http://bit.ly/2jCgJYW. Para o UBUNTU e derivados é:
* curl https://cli-assets.heroku.com/install-ubuntu.sh | sh

# CRIE URL DA APLICAÇÃO E REPOSITORIO GIT NO HEROKU PARA ENVIO DA APLICAÇÃO
* heroku apps:create nome_aplicacao

# CONFIGURE OS HOSTS PERMITIDOS NO `settings.py`
Inclua seu endereço na variável ALLOWED_HOSTS. Exemplo:
- ALLOWED_HOSTS = ['nome_aplicacao.herokuapp.com']

# INSTALE O PLUGIN DE CONFIGURAÇÃO
* heroku plugins:install heroku-config

# ENVIE AS VARIÁVEIS DE AMBIENTE .env PARA HEROKU 
* heroku config:push											

# GRAVE A VERSÃO DO PROJETO NO SEU REPOSITÓRIO LOCAL GIT 
* git add .
* git commit -m 'Configuring the app'

# ENVIE A VERSÃO DO PROJETO PARA O REPOSITÓRIO REMOTO DO HEROKU
* git push --force heroku master

# CRIE AS TABELAS NO BANDO DE DADOS COM O COMANDO REMOTO DO HEROKU
* heroku run python3 manage.py migrate

# CRIE O SUPERUSUÁRIO DA SUA APLICAÇÃO
* heroku run python3 manage.py createsuperuser

# FIM!

# CASO QUEIRA DESABILITAR O COLLECTSTATIC PARA UTILIZAR O REPOSITÓRIO DE ARQUIVOS ESTÁTICOS DA AMAZON
* heroku config:set DISABLE_COLLECTSTATIC=1

# CASO QUEIRA HABILITAR A VARIÁVEL DEBUG PARA TRUE
* heroku config:set DEBUG=True
