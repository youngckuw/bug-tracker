# Use the official Jenkins image (JDK 17)
FROM jenkins/jenkins:lts-jdk17

# Switch to root for package installation
USER root

# Install required packages and add Docker repository
RUN apt-get update && \
    DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends \
        ca-certificates \
        curl \
        git \
        lsb-release && \
    rm -rf /var/lib/apt/lists/*
RUN apt-get update && \
    apt-get install -y --no-install-recommends ca-certificates git && \
    rm -rf /var/lib/apt/lists/*
# Configure Git to skip SSL verification (if needed)
RUN git config --global http.sslVerify false

# Add Docker CE CLI
RUN curl -fsSLo /usr/share/keyrings/docker-archive-keyring.asc \
      https://download.docker.com/linux/debian/gpg && \
    echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.asc] \
          https://download.docker.com/linux/debian $(lsb_release -cs) stable" > /etc/apt/sources.list.d/docker.list && \
    apt-get update && \
    DEBIAN_FRONTEND=noninteractive apt-get install -y docker-ce-cli && \
    rm -rf /var/lib/apt/lists/*

RUN chmod 644 /etc/ssl/certs/ca-certificates.crt

COPY bundleCA.pem /usr/local/share/ca-certificates/bundleCA.crt
RUN update-ca-certificates
RUN git config --global http.sslCAInfo /usr/local/share/ca-certificates/bundleCA.crt
# Return to the Jenkins user
USER jenkins