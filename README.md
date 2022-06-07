<h1 align="center">Инструкция о том, как я разворачивал Django проект на сервере, с использование Docker + Nginx + Django соответственно</h1>
<p>Первый делом нужно обновить сервер двумя командами</p>
<li>sudo apt-get update</li>
<li>sudo apt-get upgrade</li>
<br>
Затем лично для своих нужнд, я поставил следующие пакеты
<li>sudo apt-get install -y zsh tree redis-server nginx zsh openssl libssl-dev zlib1g-dev</li>
<br>
Затем устанавливаю oh-my-zsh:
<li>sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"</li>
<br>
И перехожу к Питону:
<br>
<li>wget https://www.python.org/ftp/python/3.8.5/Python-3.8.5.tgz - качаем исходники</li>
<li>tar xvf Python-3.8.5 - разархивируем исходники</li>
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
