FROM continuumio/anaconda3:2020.02

ENV PYTHONUNBUFFERED 1

WORKDIR /usr/src/tegakihira/api

RUN apt-get update --allow-releaseinfo-change && apt-get install -y --no-install-recommends \
  net-tools \
  sudo \
  bzip2 \
  curl \
  gcc \
  git \
  python3-dev \
  vim \
  && \
  apt-get clean && \
  pip install --upgrade pip && \
  conda update -n base conda && \
  conda update --all && \
  conda clean --all -y && \
  conda install -c anaconda django && \
  conda install -c conda-forge django-filter && \
  conda install -c conda-forge djangorestframework && \
  conda install -c conda-forge djangorestframework-jwt && \
  conda install -c conda-forge django-cors-headers && \
  conda install -c anaconda python-memcached && \
  conda install -c anaconda psycopg2 && \
  conda install -c anaconda gunicorn && \
  conda install -c conda-forge dj-database-url && \
  conda install -c conda-forge django-environ

ENV DJANGO_ENV production
ADD . .

RUN python get-pip.py
RUN pip install tensorflow
RUN pip install dj3-cloudinary-storage

RUN ls
RUN python manage.py collectstatic --noinput

MAINTAINER admin
ENV USER admin
RUN useradd -m ${USER}
RUN gpasswd -a ${USER} sudo
RUN echo "${USER}:test_pass" | chpasswd
USER ${USER}

CMD gunicorn -b 0.0.0.0:$PORT api.wsgi
