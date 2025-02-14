# FROM python:3.8.10-alpine
# FROM python:3.8.10
From ubuntu:20.04
RUN aDEBIAN_FRONTEND=noninteractive apt update && DEBIAN_FRONTEND=noninteractive apt install python3-pip -y

# upgrade pip
RUN pip install --upgrade pip
# RUN pip install wheel
# RUN apt install gdal-bin python3-gdal
# get curl for healthchecks
# RUN apk add curl gdal-bin python3-gdal
#RUN apk add --no-cache gdal gdal-dev
RUN DEBIAN_FRONTEND=noninteractive apt install -y libgdal-dev gdal-bin python3.8-venv
# RUN apt-get update && apt-get install -y libgdal-dev gdal-bin
# RUN apk add --no-cache gcc g++ make
# ENV CPLUS_INCLUDE_PATH=/usr/include/gdal:$CPLUS_INCLUDE_PATH
# ENV C_INCLUDE_PATH=/usr/include/gdal:$C_INCLUDE_PATH


# permissions and nonroot user for tightened security
RUN adduser --disabled-password --disabled-login nonroot
RUN mkdir /home/app/ && chown -R nonroot:nonroot /home/app
RUN mkdir -p /var/log/flask-app && touch /var/log/flask-app/flask-app.err.log && touch /var/log/flask-app/flask-app.out.log
RUN chown -R nonroot:nonroot /var/log/flask-app
WORKDIR /home/app
USER nonroot

# copy all the files to the container
COPY --chown=nonroot:nonroot . .

# venv
ENV VIRTUAL_ENV=/home/app/venv

# python setup
RUN python3 -m venv $VIRTUAL_ENV
ENV PATH="$VIRTUAL_ENV/bin:$PATH"
RUN export FLASK_APP=app.py
RUN pip install -r requirements.txt

# define the port number the container should expose
EXPOSE 5000

CMD ["python", "app.py"]
