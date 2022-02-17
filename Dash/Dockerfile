FROM python:3.9.7

USER root

WORKDIR /app

COPY requirements.txt /tmp/requirements.txt

RUN pip3 install --upgrade pip

RUN pip3 install -r /tmp/requirements.txt

EXPOSE 8050

CMD ["python3","app.py"]