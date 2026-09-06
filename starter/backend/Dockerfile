FROM python:3.10
ENV FLASK_RUN_HOST=0.0.0.0

WORKDIR /

# 1. Create the user
RUN useradd -m -u 1000 app

# 2. Install pipenv (no apt-get needed, C-compilers are already in this image!)
RUN pip install pipenv

# 3. Setup directories
RUN mkdir -p /var/www/app && mkdir /app && chown -R app:app /var/www/app /app

# 4. Install Pipenv dependencies
COPY Pipfile* /app/
RUN cd /app && pipenv install --system

# 5. Copy the app
COPY . /app/

# 6. Start the server
CMD ["uwsgi", "--http", ":5000", "--master", "--enable-threads", "-s", "/var/www/app/app.sock", "--manage-script-name", "--mount", "/=app:app"]