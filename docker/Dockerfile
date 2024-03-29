# Dockerfile
FROM python:3.9-slim-buster

# Install Java
COPY --from=openjdk:8-jre-slim /usr/local/openjdk-8 /usr/local/openjdk-8
ENV JAVA_HOME /usr/local/openjdk-8
RUN update-alternatives --install /usr/bin/java java /usr/local/openjdk-8/bin/java 1

WORKDIR /usr/app

# Install Linux Packages
RUN apt-get update && \
    apt-get upgrade -y && \
    apt-get install -y --no-install-recommends \
    wget \
    snapd \
    git \
    curl \
    sshpass \
    openssh-server \ 
    sudo \
    nmap \
    gnupg \
    kafkacat \
    lsb-release \ 
    alien \
    apt-transport-https \
    python-dev \
    libpq-dev \
    ca-certificates && \
    apt-get purge -y --auto-remove

RUN apt install -y build-essential

# Install Kafka Binary
RUN curl "https://downloads.apache.org/kafka/3.5.1/kafka_2.12-3.5.1.tgz" -o /tmp/kafka.tgz
RUN mkdir /kafka
RUN tar -xvzf /tmp/kafka.tgz --directory /kafka --strip-components 1
RUN rm -f /tmp/kafka.tgz

# Install Rust
RUN curl https://sh.rustup.rs -sSf | sh -s -- -y
ENV PATH="/root/.cargo/bin:${PATH}"

# Install Kafka Topic Analyzer
ENV USER=root
RUN cargo install kafka-topic-analyzer
RUN cp /root/.cargo/bin/kafka-topic-analyzer /usr/bin
RUN chmod 755 /usr/bin/kafka-topic-analyzer

# Install Python packages
COPY requirements.txt ./
RUN pip install --upgrade pip
RUN pip install --no-cache-dir -r requirements.txt

RUN pip install sqlfluff
RUN pip install semgrep
RUN pip install checkov
RUN pip install sqlfluff-templater-dbt
RUN pip install shandy-sqlfmt[jinjafmt]

# Install mage-ai
RUN pip install mage-ai
RUN pip uninstall -y pyOpenSSL

# Install snowflake-snowsql
RUN curl "https://sfc-repo.snowflakecomputing.com/snowsql/bootstrap/1.2/linux_x86_64/snowflake-snowsql-1.2.27-1.x86_64.rpm" -o /tmp/snowflake-snowsql.rpm
RUN alien -i /tmp/snowflake-snowsql.rpm

# Install teraform
RUN wget -qO - terraform.gpg https://apt.releases.hashicorp.com/gpg | sudo gpg --dearmor -o /usr/share/keyrings/terraform-archive-keyring.gpg
RUN echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/terraform-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main" > /etc/apt/sources.list.d/terraform.list
RUN apt update
RUN apt install terraform

# Install Java
RUN apt install -y default-jre
RUN apt install -y default-jdk

# Install npm
RUN curl -sL https://deb.nodesource.com/setup_14.x | bash -
RUN apt-get install -y nodejs
RUN apt-get install gcc g++ make

# Upgrade dbt 
RUN pip install dbt-core dbt-postgres dbt-redshift dbt-snowflake dbt-bigquery dbt-materialize dbt-databricks --upgrade

# Create ubuntu user 
RUN useradd -rm -d /home/ubuntu -s /bin/bash -g root -G sudo -u 1001 ubuntu 
USER ubuntu
WORKDIR /home/ubuntu

# Create alias for dbt-set-profile
RUN echo "alias dbt-set-profile='unset DBT_PROFILES_DIR && export DBT_PROFILES_DIR=\$PWD'" >> ~/.bashrc
