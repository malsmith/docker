FROM anapsix/alpine-java:7_jdk
MAINTAINER Felipe Sabino <felipe@sabino.pro>

RUN apk update && \
	apk add gcc g++ make unzip git --no-cache && \
	rm -fr /var/cache/apk/*

ENV MAVEN_VERSION 3.3.9
ENV MAVEN_HOME /usr/lib/mvn
ENV PATH $MAVEN_HOME/bin:$PATH

RUN wget http://archive.apache.org/dist/maven/maven-3/$MAVEN_VERSION/binaries/apache-maven-$MAVEN_VERSION-bin.tar.gz && \
	tar -zxvf apache-maven-$MAVEN_VERSION-bin.tar.gz && \
	rm apache-maven-$MAVEN_VERSION-bin.tar.gz && \
	mv apache-maven-$MAVEN_VERSION /usr/lib/mvn

RUN apk update && apk add wget ca-certificates

RUN cd /tmp \
    && wget http://github.com/google/protobuf/releases/download/v2.5.0/protobuf-2.5.0.zip \
    && unzip protobuf-2.5.0.zip \
    && cd protobuf-2.5.0 \
    && ./configure --prefix=/usr --disable-shared \
    && make && make install

RUN git clone https://github.com/dremio/dremio-oss.git /dremio

RUN cd /dremio && \
	mvn clean install -DskipTests

WORKDIR /dremio

EXPOSE 9047 31010

CMD cd /dremio && mvn compile exec:exec -pl dac/daemon