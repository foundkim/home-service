FROM python:3.8-buster

# copy source and install dependencies
RUN mkdir -p /opt/app
RUN mkdir -p /opt/app/pip_cache
RUN mkdir -p /opt/app/api
COPY Pipfile.lock start-server.sh /opt/app/
COPY api /opt/app/api/
WORKDIR /opt/app
RUN mkdir -p mediafiles staticfiles
RUN cat Pipfile.lock \
            | grep -B1 '"hashes"\|"version": ' \
            | grep -v '"markers": \|"hashes": ' \
            | grep ": {\|version" \
            | sed -e 's/: {$//g' \
            | tr '\n' ',' | tr -s ' ' ' ' \
            | sed -e 's/, "version": "//g;s/", "/ /g;s/"//g;s/,//g' \
            | tr ' ' '\n' \
            | grep -v "^$" > requirements.txt
RUN pip install -r requirements.txt --cache-dir /opt/app/pip_cache
RUN chown -R www-data:www-data /opt/app

# start server
EXPOSE 8010
STOPSIGNAL SIGTERM
CMD ["/opt/app/start-server.sh"]