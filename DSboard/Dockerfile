FROM python:3.7

EXPOSE 8501

WORKDIR /app

COPY requirements.txt /tmp/requirements.txt

RUN pip3 install -r /tmp/requirements.txt

CMD streamlit run app.py