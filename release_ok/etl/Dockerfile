FROM python:3.8-slim

WORKDIR /app

COPY etl.py /app/etl.py

RUN pip install requests

CMD ["python", "/app/etl.py"]
