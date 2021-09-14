FROM python:3.9-alpine3.13

COPY . .

RUN pip3 install -r requirements.txt

LABEL maintainer = "Niklas Nielsen"

CMD ["python3", "app.py"]

