FROM ubuntu:18.04

RUN apt-get update && apt-get -y install python3 python3-pip python3-dev git sed vim 
RUN pip3 install Django==2.1.* uwsgi

RUN git clone https://github.com/jruizvar/django-tutorial.git


COPY ./create_user.py ./django-tutorial

WORKDIR ./django-tutorial

# Disable if running in Production
#RUN sed -i "s/DEBUG = True/DEBUG = False/g" mysite/settings.py 

RUN sed -i "s/ALLOWED_HOSTS = \[\]/ALLOWED_HOSTS = \['*'\]/g" mysite/settings.py
RUN echo "STATIC_ROOT = '/django-tutorial/static'" >> mysite/settings.py
RUN echo "STATIC_URL = '/static/'" >> mysite/settings.py

RUN python3 manage.py migrate
RUN python3 manage.py collectstatic
RUN cat create_user.py | python3 manage.py shell



EXPOSE 80
CMD ["uwsgi", "--http", ":80", "--static-map", "/static=/django-tutorial/static",  "--module", "mysite.wsgi"]


