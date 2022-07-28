<h1 align="center">Инструкция о том, как я разворачивал Django проект на сервере, с использование Docker + Nginx + Gunicorn + Django соответственно</h1>
<p>Первый делом нужно обновить сервер двумя командами</p>
<li>sudo apt-get update</li>
<li>sudo apt-get upgrade</li>
<br>
Затем лично я поставил следующие пакеты, для нужд и для питона
<li>sudo apt-get install -y zsh tree redis-server nginx gunicorn postgresql-13 zsh openssl libssl-dev musl-dev postgresql-server-dev-all libpq-dev zlib1g-dev libffi-dev python3.8-dev libsqlite3-dev libxslt-dev curl git gcc
</li>
<br>
Затем устанавливаю oh-my-zsh:
<li>sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"</li>
<li>chsh -s $(which zsh)</li>
<br>
И перехожу к Питону:
<br>
<li>wget https://www.python.org/ftp/python/3.8.5/Python-3.8.5.tgz - качаем исходники</li>
<li>tar xvf Python-3.8.5.tgz - разархивируем исходники</li>
<li>cd Python-3.8.5 - обязательно переходим в дирректорию</li>
<li>mkdir ~/.python - создаем дирректорию для python</li>
<li>./configure --enable-optimizations --prefix=/root/.python - конфигурируем python и в качестве дирректории указываем ту дирректорию, в которую будем его ставить</li>
<li>make -j* - где *-количество ядер на сервере</li>
<li>sudo make altinstall - две команды для установки</li>
<br>
Затем проапргрейдим PIP:
<li>/root/.python/bin/python3.8 -m pip install -U pip</li>
<br>
Теперь осталось скачать проект с GitHub и приступить к дальнейшей настройке:
<li>git clone https://github.com/YOUR-USERNAME/YOUR-REPOSITORY folder_name</li>
<p>P.S. Чтобы клонировать реппозиторий на сервере через SSH, нужно предварительно создать публичный ключ (cd ~/.ssh --> ssh-keygen -o --> cat ~/.ssh/id_rsa.pub) и закинуть его в настройки профиля на GitHub</p>

<h1 align="center">Следующий этап - настройка необходимого окружения для Django</h1>
Первым делом, следует указать zsh, где находится путь до Python, для этого командой nano ~/.zshrc откроем файл на редактирование и вобъем следующие строки export PATH=$PATH:/root/.python/bin, сохраним и перезапустим командой source ~/.zshrc
<p>Командой python3.8 -m venv env создадим виртуальной окружение. Активируем его командой . env/bin/activate</p>

<h1 align="center">PGSQL (Просто скопировал настройки из проекта с супервизором без докера)</h1>
Install PostgreSQL 11 and configure locales.

wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add - ; \
RELEASE=$(lsb_release -cs) ; \
echo "deb http://apt.postgresql.org/pub/repos/apt/ ${RELEASE}"-pgdg main | sudo tee  /etc/apt/sources.list.d/pgdg.list ; \
sudo apt update ; \
sudo apt -y install postgresql-11 ; \
sudo localedef ru_RU.UTF-8 -i ru_RU -fUTF-8 ; \
export LANGUAGE=ru_RU.UTF-8 ; \
export LANG=ru_RU.UTF-8 ; \
export LC_ALL=ru_RU.UTF-8 ; \
sudo locale-gen ru_RU.UTF-8 ; \
sudo dpkg-reconfigure locales
Add locales to /etc/profile:

sudo vim /etc/profile
    export LANGUAGE=ru_RU.UTF-8
    export LANG=ru_RU.UTF-8
    export LC_ALL=ru_RU.UTF-8
Change postges password, create clear database named dbms_db:

sudo passwd postgres
su - postgres
export PATH=$PATH:/usr/lib/postgresql/11/bin
createdb --encoding UNICODE dbms_db --username postgres
exit
Create dbms db user and grand privileges to him:

sudo -u postgres psql
postgres=# ...
create user dbms with password 'some_password';
ALTER USER dbms CREATEDB;
grant all privileges on database dbms_db to dbms;
\c dbms_db
GRANT ALL ON ALL TABLES IN SCHEMA public to dbms;
GRANT ALL ON ALL SEQUENCES IN SCHEMA public to dbms;
GRANT ALL ON ALL FUNCTIONS IN SCHEMA public to dbms;
CREATE EXTENSION pg_trgm;
ALTER EXTENSION pg_trgm SET SCHEMA public;
UPDATE pg_opclass SET opcdefault = true WHERE opcname='gin_trgm_ops';
\q
exit
Now we can test connection. Create ~/.pgpass with login and password to db for fast connect:

vim ~/.pgpass
	localhost:5432:dbms_db:dbms:some_password
chmod 600 ~/.pgpass
psql -h localhost -U dbms dbms_db
Run SQL dump, if you have:

psql -h localhost dbms_db dbms  < dump.sql


<h1 align="center">Docker</h1>
Установим Docker и Docker-Compose так, как указано в официальной документации https://docs.docker.com/engine/install/. 



