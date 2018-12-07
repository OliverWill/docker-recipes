FROM python:3.6.5

# NOTE: debian version might change, used to work with .../debian/8/... in the past but not any more
RUN apt-get -y update \
	&& apt-get -y install apt-transport-https \
	&& apt-get -y install sudo \
	&& apt-get -y install curl \
	&& curl https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add - \
	&& curl https://packages.microsoft.com/config/debian/9/prod.list | sudo tee /etc/apt/sources.list.d/mssql-tools.list \
	&& sudo apt-get -y update \
	&& sudo ACCEPT_EULA=Y apt-get -y install mssql-tools \
	&& sudo apt-get -y install unixodbc-dev \
	&& echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bash_profile \
	&& echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc \
	&& . ~/.bashrc \
	&& apt-get -y install locales \
	&& echo "en_US.UTF-8 UTF-8" > /etc/locale.gen \
	&& locale-gen \
	&& sudo apt-get -y install python3 python3-pip gcc g++ build-essential

WORKDIR /app/

COPY requirements.txt /app/
RUN pip install -r ./requirements.txt

COPY app.py __init__.py pyodbctest.py /app/

# ENTRYPOINT /bin/bash
EXPOSE 5000

ENTRYPOINT python ./app.py
