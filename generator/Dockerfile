FROM golang:latest

RUN apt-get update && apt-get install -y libsnmp-dev p7zip-full unzip

COPY Makefile /go/src/Makefile
RUN cd /go/src && make mibs MIBDIR=/opt/mibs

COPY *.go /go/src/generator/
RUN cd /go/src/generator && go get -v . && go install

# Create required folders so we avoid warnings from NET-SNMP
RUN mkdir -p /usr/share/snmp /var/lib/snmp/mib_indexes

WORKDIR "/opt"

ENTRYPOINT ["/go/bin/generator"]

ENV MIBDIRS mibs
ENV SNMPCONFPATH /usr/share/snmp/
COPY snmp.conf /usr/share/snmp/snmp.conf

CMD ["generate"]
