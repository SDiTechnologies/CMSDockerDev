FROM python:3.9-alpine

ENV PYTHONUNBUFFERED 1
ENV PROJ demo
# COPY ./requirements.txt /requirements.txt

RUN apk add --update --no-cache postgresql-client jpeg-dev;

RUN apk add --update --no-cache --virtual .tmp-build-deps \ 
    build-base \
    gcc \
    curl \
    git \
    libc-dev \
    linux-headers \
    postgresql-dev \
    musl-dev \
    zlib \
    zlib-dev \
    bash;

ADD requirements/ /requirements/

RUN python -m venv /venv \
    && /venv/bin/pip install -U pip \
    && /venv/bin/pip install --no-cache-dir -r /requirements/prod.txt \
    && apk del .tmp-build-deps

# RUN pip install --no-cache-dir -r /requirements/prod.txt

# RUN apk del .tmp-build-deps

RUN mkdir /app
WORKDIR /app
ADD . /app/
EXPOSE 8000

# Add custom environment variables needed by Django or your settings file here:
ENV DJANGO_SETTINGS_MODULE=$PROJ.settings.production DJANGO_DEBUG=off

# Tell uWSGI where to find your wsgi file:
ENV UWSGI_WSGI_FILE=$PROJ/wsgi.py

# Base uWSGI configuration (you shouldn't need to change these):
ENV UWSGI_VIRTUALENV=/venv UWSGI_HTTP=:8000 UWSGI_MASTER=1 UWSGI_HTTP_AUTO_CHUNKED=1 UWSGI_HTTP_KEEPALIVE=1 UWSGI_UID=1000 UWSGI_GID=2000 UWSGI_LAZY_APPS=1 UWSGI_WSGI_ENV_BEHAVIOR=holy

# Number of uWSGI workers and threads per worker (customize as needed):
ENV UWSGI_WORKERS=2 UWSGI_THREADS=4

# uWSGI uploaded media file serving configuration:
ENV UWSGI_STATIC_MAP="/media/=/app/$PROJ/media/"

# Call collectstatic with dummy environment variables:
RUN DATABASE_URL=postgres:///none REDIS_URL=none /venv/bin/python manage.py collectstatic --noinput

# make sure static files are writable by uWSGI process
RUN mkdir -p /app/$PROJ/media/images && chown -R 1000:2000 /app/$PROJ/media

# mark the destination for images as a volume
VOLUME ["/app/$PROJ/media/images/"]

# start uWSGI, using a wrapper script to allow us to easily add more commands to container startup:
ENTRYPOINT ["/app/docker-entrypoint.sh"]

# Start uWSGI
CMD ["/venv/bin/uwsgi", "--show-config"]

