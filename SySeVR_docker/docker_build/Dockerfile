# Use a more recent base image
FROM tensorflow/tensorflow:latest-gpu-py3

# Set environment variables
ENV JAVA_HOME=/usr/java/jdk1.8.0_161
ENV ANT_HOME=/usr/ant/apache-ant-1.9.14
ENV PATH=$PATH:$JAVA_HOME/bin:$ANT_HOME/bin

# Copy home
COPY ./home/ /home/

# Copy Implementation
COPY ./Implementation/ /home/SySeVR/Implementation/

# Copy necessary files
COPY ./home/SySeVR/softdir /tmp/softdir

# Remove NVIDIA repositories and update
RUN rm -f /etc/apt/sources.list.d/cuda.list && \
    rm -f /etc/apt/sources.list.d/nvidia-ml.list && \
    sed -i '/developer.download.nvidia.com/d' /etc/apt/sources.list && \
    apt-key del 7fa2af80 && \
    apt-get update && \
    apt-get install -y --no-install-recommends \
        python-setuptools \
        python-dev \
        python-pip \
        graphviz \
        libgraphviz-dev \
        pkg-config \
        python-igraph \
        python-virtualenv && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# Set up Java and Ant
RUN mkdir -p /usr/java /usr/ant && \
    mv /tmp/softdir/jdk1.8.0_161 /usr/java/ && \
    mv /tmp/softdir/apache-ant-1.9.14 /usr/ant/

# Set up custom sources and profile
RUN mv /tmp/softdir/sources.list /etc/apt/ && \
    rm -rf /etc/apt/sources.list.d && \
    mv /tmp/softdir/profile /etc/profile

# Run environment setup script
RUN cd /tmp/softdir && \
    chmod +x env.sh && \
    ./env.sh

# Install Python packages
RUN cd /tmp/softdir/py2neo-py2neo-2.0 && \
    python2 setup.py install && \
    cd /tmp/softdir/python-joern-0.3.1 && \
    python2 setup.py install

RUN pip3 install --no-cache-dir \
    xlrd \
    gensim==3.4 \
    pyyaml

# Clean up
RUN rm -rf /tmp/softdir

# Set working directory
WORKDIR /home/SySeVR

# Command to run when starting the container
CMD ["/bin/bash"]