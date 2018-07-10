# Copyright 2017-2017 Amazon.com, Inc. or its affiliates. All Rights Reserved.
#
# Licensed under the Amazon Software License (the "License"). You may not use this file except in compliance with the License.
# A copy of the License is located at
#
#    http://aws.amazon.com/asl/
#
# or in the "license" file accompanying this file.
# This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, express or implied.
# See the License for the specific language governing permissions and limitations under the License.
#

FROM ubuntu:bionic

ENV DEBIAN_FRONTEND=noninteractive

# Install git, SSH, and other utilities
RUN set -ex \
    && echo 'Acquire::CompressionTypes::Order:: "gz";' > /etc/apt/apt.conf.d/99use-gzip-compression \
    && apt-get update \
    && apt-get install software-properties-common -y --no-install-recommends \
    && apt-get install git -y --no-install-recommends \
    && git version \
    && apt-get install -y --no-install-recommends openssh-client \
    && mkdir ~/.ssh \
    && touch ~/.ssh/known_hosts \
    && ssh-keyscan -t rsa,dsa -H github.com >> ~/.ssh/known_hosts \
    && ssh-keyscan -t rsa,dsa -H bitbucket.org >> ~/.ssh/known_hosts \
    && chmod 600 ~/.ssh/known_hosts \
    && apt-get install -y --no-install-recommends \
       wget curl tzdata sudo ca-certificates \
       unzip \
    && rm -rf /var/lib/apt/lists/* \
    && apt-get clean

ENV JAVA_VERSION=8 \
    JAVA_HOME="/usr/lib/jvm/java-8-oracle" \
    ANT_VERSION=1.9.6 \
    MAVEN_VERSION=3.3.3 \
    MAVEN_HOME="/usr/share/maven" \
    MAVEN_CONFIG="/root/.m2" \
    GRADLE_VERSION=2.7

# Install Java
RUN set -ex \
    && apt-get update \
    && echo oracle-java8-installer shared/accepted-oracle-license-v1-1 select true | debconf-set-selections \
    && add-apt-repository ppa:webupd8team/java \
    && apt-get update \
    && apt-get -y install oracle-java8-installer \
    && apt-get clean \
    && mkdir -p /usr/src/ant \
    && wget "http://archive.apache.org/dist/ant/binaries/apache-ant-$ANT_VERSION-bin.tar.gz" -O /usr/src/ant/apache-ant-$ANT_VERSION-bin.tar.gz \
    && tar -xzf /usr/src/ant/apache-ant-$ANT_VERSION-bin.tar.gz -C /usr/local \
    && ln -s /usr/local/apache-ant-$ANT_VERSION/bin/ant /usr/bin/ant \
    && rm -rf /usr/src/ant \
    && mkdir -p /usr/share/maven /usr/share/maven/ref $MAVEN_CONFIG \
    && curl -fsSL "https://archive.apache.org/dist/maven/maven-3/$MAVEN_VERSION/binaries/apache-maven-$MAVEN_VERSION-bin.tar.gz" \
        | tar -xzC /usr/share/maven --strip-components=1 \
    && ln -s /usr/share/maven/bin/mvn /usr/bin/mvn \
    && mkdir -p /usr/src/gradle \
    && wget "https://services.gradle.org/distributions/gradle-$GRADLE_VERSION-bin.zip" -O /usr/src/gradle/gradle-$GRADLE_VERSION-bin.zip \
    && unzip /usr/src/gradle/gradle-$GRADLE_VERSION-bin.zip -d /usr/local \
    && ln -s /usr/local/gradle-$GRADLE_VERSION/bin/gradle /usr/bin/gradle \
    && rm -rf /usr/src/gradle \
    && rm -fr /var/lib/apt/lists/* /tmp/* /var/tmp/*

COPY m2-settings.xml $MAVEN_CONFIG/settings.xml
