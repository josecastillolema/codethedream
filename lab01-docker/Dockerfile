FROM python:2

MAINTAINER Jose Castillo Lema <josecastillolema@gmail.com>

ADD api.py requirements.txt /
RUN pip install -r ./requirements.txt

EXPOSE 5000
HEALTHCHECK CMD curl --fail http://localhost:5000/ || exit 1

CMD [ "./api.py" ]
