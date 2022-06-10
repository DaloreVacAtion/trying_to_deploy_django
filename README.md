<h1 align="center">Инструкция о том, как я разворачивал Django проект на сервере, с использование Docker + Nginx + Gunicorn + Django соответственно</h1>
<p>Первый делом нужно обновить сервер двумя командами</p>
<li>sudo apt-get update</li>
<li>sudo apt-get upgrade</li>
<br>
Затем лично я поставил следующие пакеты, для нужд и для питона
<li>sudo apt-get install -y zsh tree redis-server nginx gunicorn postgresql-13 zsh openssl libssl-dev postgresql-server-dev-all libpq-dev zlib1g-dev libffi-dev python3.8-dev libsqlite3-dev</li>
<br>
Затем устанавливаю oh-my-zsh:
<li>sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"</li>
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
<p>Командой python3.8 -m venv env создадим виртуальной окружение. Активируем его командой . env/bin/activate и утсановим зависимости командой pip install -r requirements.txt</p>

