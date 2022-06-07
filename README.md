<h1 align="center">Инструкция о том, как я разворачивал Django проект на сервере, с использование Docker + Nginx + Django соответственно</h1>
  <p>Первый делом нужно обновить сервер двумя командами</p>
<li>sudo apt-get update</li>
<li>sudo apt-get upgrade</li>
  
Затем лично для своих нужнд, я поставил следующие пакеты
<li>sudo apt-get install -y zsh tree redis-server nginx zsh openssl libssl-dev zlib1g-dev</li>

Затем устанавливаю oh-my-zsh:
<li>sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"</li>
И перехожу к Питону:
wget https://www.python.org/ftp/python/3.8.5/Python-3.8.5.tgz - качаем исходники
tar xvf Python-3.8.5 - разархивируем исходники
cd Python-3.8.5 - обязательно переходим в дирректорию
mkdir ~/.python - создаем дирректорию для python
./configure --enable-optimizations --prefix=/root/.python - конфигурируем python и в качестве дирректории указываем ту дирректорию, в которую будем его ставить
make -j* - где *-количество ядер на сервере
sudo make altinstall - две команды для установки

Затем проапргрейдим PIP:
<li>/root/.python/bin/python3.8 -m pip install -U pip</li>

Теперь осталось скачать проект с GitHub и приступить к дальнейшей настройке:
