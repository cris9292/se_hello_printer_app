Simple Flask App 17.02.2018
================

Aplikacja Dydaktyczna wyświetlająca imię i wiadomość w różnych formatach dla zajęć
o Continuous Integration, Continuous Delivery i Continuous Deployment.

- Rozpocząnając pracę z projektem (wykorzystując virtualenv). Hermetyczne środowisko dla pojedyńczej aplikacji w python-ie:

  ::

    source /usr/bin/virtualenvwrapper.sh
    mkvirtualenv wsb-simple-flask-app
    # jezeli nie zadziała trzeba zainstalwac komponenty z sekcji pomocnicze.
    pip install -r requirements.txt
    pip install -r test_requirements.txt

- Uruchamianie applikacji:

  ::

    # jako zwykły program
    python main.py

    # albo:
    PYTHONPATH=. FLASK_APP=hello_world flask run

- Uruchamianie testów (see: http://doc.pytest.org/en/latest/capture.html):

  ::

    PYTHONPATH=. py.test
    PYTHONPATH=. py.test  --verbose -s

- Kontynuując pracę z projektem, aktywowanie hermetycznego środowiska dla aplikacji py:

  ::

    source /usr/bin/virtualenvwrapper.sh
    workon wsb-simple-flask-app


- Integracja z TravisCI:

# Tworzymy plik konfiguracyjny dla dolonego jezyka  " .travis.yml "
Dla pythona dostepne pod adresem https://docs.travis-ci.com/user/languages/python/
...
language: python
services:
  - docker
python:
  - "2.7"

install:
  - make deps

script:
  - make test  ### Docker opcjonalnie

after_success:
  - make docker_build
  - make docker_push
...

- Eksport dockera do hub.docker https://hub.docker.com/

Dodajemy do Makefile sekcje docker_push od dockera.
...
USERNAME=cris9292
TAG=$(USERNAME)/hello-world-printer

docker_push : docker_build
	@docker login --username $(USERNAME) --password $${DOCKER_PASSWORD}; \
	docker tag hello-world-printer $(TAG); \
	docker push $(TAG); \
	docker logout;
...
Pomocnicze pliki instalcyjne
==========

- Instalacja python virtualenv i virtualenvwrapper:

  ::

    yum install python-pip
    pip install -U pip
    pip install virtualenv
    pip install virtualenvwrapper

- Instalacja docker-a:

  ::

    yum remove docker \
        docker-common \
        container-selinux \
        docker-selinux \
        docker-engine

    yum install -y yum-utils

    yum-config-manager \
      --add-repo \
      https://download.docker.com/linux/centos/docker-ce.repo

    yum makecache fast
    yum install docker-ce
    systemctl start docker

- Instalacja bash it (opcjonalnie )

    git clone --depth=1 https://github.com/Bash-it/bash-it ~/.bash-it
    cd ~/.bash-it
    ./install.sh

- Konfiguracja gita

- sprawdź konfigurację (małe L):
git config -l
git config --global user.name "wojciech11"
- nie lubimy spamerów:
git config --global user.email "wojciech11@users.noreply.github.com"
- domyślnie jest vim albo emacs
git config --global core.editor "atom --wait

- Dodanie monitoringu Statuscake oraz TravisCI

Statuscake

.. image:: https://app.statuscake.com/button/index.php?Track=xtMhzQt0gU&Days=1&Design=1
    :target: https://www.statuscake.com

TravisCI

.. image:: https://travis-ci.org/cris9292/se_hello_printer_app.svg?branch=master
    :target: https://travis-ci.org/cris9292/se_hello_printer_app

Materiały
=========

- https://virtualenvwrapper.readthedocs.io/en/latest/
