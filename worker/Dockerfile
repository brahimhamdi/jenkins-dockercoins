FROM 	python:alpine
MAINTAINER      brahim.hamdi.consult@gmail.com
RUN 	pip install redis
RUN 	pip install requests
COPY 	worker.py /
CMD 	["python", "worker.py"]
