FROM python:3.10-slim-bullseye
ENV FLASK_RUN_HOST=0.0.0.0

WORKDIR /

# 1. Create the user using Debian syntax
RUN useradd -m -u 1000 app

# 2. Install build dependencies (Bullseye still supports libpcre3-dev)
RUN apt-get update && \
    apt-get install -y \
    build-essential \
    libpcre3 \
    libpcre3-dev \
    && rm -rf /var/lib/apt/lists/* \
    && pip install pipenv

# 3. Setup directories
RUN mkdir -p /var/www/app && mkdir /app && chown -R app:app /var/www/app /app

# 4. Install Pipenv dependencies
COPY Pipfile* /app/
RUN cd /app && pipenv install --system

# 5. Copy the app
COPY . /app/

# 6. Start the server
CMD ["uwsgi", "--http", ":5000", "--master", "--enable-threads", "-s", "/var/www/app/app.sock", "--manage-script-name", "--mount", "/=app:app"]