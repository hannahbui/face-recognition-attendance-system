FROM debian@sha256:5d43e02a04b54189d0be8b336b694d32bfb3394ca45d507b2dff6f2889dc4285 as base

# Set the working directory in the container
WORKDIR /app
RUN mkdir -p /app/static/images

# set up env
RUN apt-get update && apt-get -y install --no-install-recommends \
	wget \
	ca-certificates

# install py 3.10.11
RUN apt update -y &&  apt upgrade -y && \
    apt-get install  -y wget build-essential checkinstall  libreadline8  libncursesw5-dev  libssl-dev  libsqlite3-dev tk-dev libgdbm-dev libc6-dev libbz2-dev libffi-dev zlib1g-dev && \
    cd /usr/src && \
     wget --no-check-certificate https://www.python.org/ftp/python/3.10.11/Python-3.10.11.tgz && \
     tar xzf Python-3.10.11.tgz && \
    cd Python-3.10.11 && \
     ./configure --enable-optimizations && \
     make install

#ssl shit
RUN python3 -m pip install --upgrade certifi
RUN update-ca-certificates --fresh
RUN export SSL_CERT_DIR=/etc/ssl/certs

# Copy the application files
COPY app.py face_detector.py GhostFaceNet_W1.3_S1_ArcFace.h5 /app/
COPY __pycache__ /app/__pycache__   
COPY static /app/static
COPY vector.index /app/vector.index
COPY utils.py /app/utils.py

# Install dependencies
COPY requirements.txt /app/
RUN pip3 install --break-system-packages --no-cache-dir -r requirements.txt

# Expose the port the app runs on
EXPOSE 8000

# Command to run the application
CMD ["python3", "app.py"]

