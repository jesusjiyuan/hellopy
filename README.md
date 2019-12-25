# hellopy
docker 化
	app:django项目
	Dockerfile
		FROM python:3.6.4

		RUN mkdir /code \
		&&apt-get update \
		&&apt-get -y install freetds-dev \
		&&apt-get -y install unixodbc-dev
		COPY app /code 
		COPY requirements.txt /code
		RUN pip install -r /code/requirements.txt -i https://pypi.douban.com/simple
		WORKDIR /code

		CMD ["/bin/bash","run.sh"]
	requirements.txt是项目运行所需要的python库
		Django
		djangorestframework
		pyDes
		PyMySQL
		redis
		requests
		pymssql
		pyodbc
		paramiko
		psutil
	run.sh是运行容器时需要调用的shell脚本
		python /code/app/manage.py runserver 0.0.0.0:8000
		
编写docker-compose.yml文件
	version: '3'
	services:
		web1:
			build: .
			image: web1
			ports:
				- "7500:8000"
			volumes: 
				- /mydate/py/webtest:/code
			privileged: true
			restart: always


docker bulid -t  webtest .  命令构建一个名字为 webtest 的镜像，

启动容器，运行刚才构建的镜像。
docker run -it -p 6500:8000 -v /mydate/py/webtest:/code --name web --restart always --privileged=true webtest

