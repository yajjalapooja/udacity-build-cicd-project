FROM python:3.10-slim-bookworm

ENV FLASK_RUN_HOST=0.0.0.0

WORKDIR /

RUN adduser --uid 1000 --disabled-password --gecos "" app

RUN apt-get update && \
    apt-get install -y \
    gcc \
    build-essential \
    libpcre3 \
    libpcre3-dev \
    libssl-dev \
    libffi-dev \
    && pip install pipenv \
    && rm -rf /var/lib/apt/lists/*

RUN mkdir -p /var/www/app && mkdir /app

COPY Pipfile* /app

RUN cd /app && pipenv install --system --deploy

COPY . /app

CMD uwsgi --http :5000 --master --enable-threads -s /var/www/app/app.sock --manage-script-name --mount /=app:app