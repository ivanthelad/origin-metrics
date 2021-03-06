#
# Copyright 2014-2015 Red Hat, Inc. and/or its affiliates
# and other contributors as indicated by the @author tags.
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#    http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
#

# Heapster Docker Base Image
FROM centos:centos7

# The image is maintained by the Hawkular Metrics team
MAINTAINER Hawkular Metrics <hawkular-dev@lists.jboss.org>

EXPOSE 8082
# original commit
#ENV HEAPSTER_COMMIT=af4752e
## based on release Heapster v0.19.0 . might be effected by this issue https://github.com/kubernetes/heapster/pull/749
#ENV HEAPSTER_COMMIT=7240063
# commit with only riemann sink and none of issus outlined 063ce38
ENV HEAPSTER_COMMIT=c791088
# commit where  go deps clean up was added that resulted in a bunch of auth issues 
#ENV HEAPSTER_COMMIT=063ce38

RUN yum install -y -q go git wget make && \
    yum clean all && \
    export GOPATH=/tmp/gopath && \
    export PATH=$PATH:$GOPATH/bin && \
    mkdir -p $GOPATH && \
    cd $GOPATH && \
    go get -d k8s.io/heapster && \
    cd src/k8s.io/heapster && \
    git checkout $HEAPSTER_COMMIT && \
    make && \
    cp heapster /opt && \
    rm -rf $GOPATH && \
    yum remove -y -q go git wget make
    
ENV PATH=$PATH:/opt

ENTRYPOINT ["/opt/heapster"]
