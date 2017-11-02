FROM centos
MAINTAINER Terence Gatt
EXPOSE 80
RUN yum update -y && yum install epel-release -y && yum install nginx uwsgi git python-pip uwsgi-plugin-python supervisor -y
RUN pip install --upgrade pip
RUN pip install django django-chartit psycopg2
RUN cd /opt && git clone https://github.com/hydramt/django-investments.git
RUN sed -i 's/localhost/10\.8\.0\.12/g' /opt/django-investments/investments/settings.py
RUN mkdir -p /opt/uwsgi/run
RUN mkdir -p /opt/uwsgi/log
RUN mkdir -p /opt/uwsgi/socket
RUN mkdir -p /opt/supervisord/log
ADD portfolios_com_mt.ini /opt/uwsgi/portfolios_com_mt.ini
ADD nginx.conf /etc/nginx/nginx.conf
ADD supervisord.conf /opt/supervisord/supervisord.conf
RUN chown -R nginx /opt
RUN python /opt/django-investments/manage.py collectstatic
CMD ["/usr/bin/supervisord", "-c", "/opt/supervisord/supervisord.conf"]
