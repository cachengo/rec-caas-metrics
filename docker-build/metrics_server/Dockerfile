# Copyright 2019 Nokia
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

FROM alpine:3.9
MAINTAINER Szekeres Balazs <balazs.szekeres@nokia.com>

ARG DEPENDENCY_MANAGER
ENV DEPENDENCY_VERSION=$DEPENDENCY_MANAGER
ARG METRICS_SERVER
ENV METRICS_SERVER_VERSION=$METRICS_SERVER
ENV GOPATH /build
ENV PATH /usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin

ENV VERSION_LDFLAGS='-extldflags "-static"'

RUN apk add --no-cache --virtual .build-deps build-base go godep curl tar git bash mercurial \
&&  if test "$(uname -m)" = "aarch64" ; then ARCH="arm64"; else ARCH="amd64"; fi \ 
&&  curl -fsSL -o /usr/local/bin/dep https://github.com/golang/dep/releases/download/v${DEPENDENCY_VERSION}/dep-linux-${ARCH} \
&&  chmod +x /usr/local/bin/dep \
&&  mkdir -p ${GOPATH}/src/github.com/kubernetes-incubator/metrics-server/ \
&&  curl -fsSL -k https://github.com/kubernetes-incubator/metrics-server/archive/v${METRICS_SERVER_VERSION}.tar.gz | tar zx --strip-components=1 -C ${GOPATH}/src/github.com/kubernetes-incubator/metrics-server/ \
&&  cd ${GOPATH}/src/github.com/kubernetes-incubator/metrics-server/ \
&&  dep ensure --vendor-only \
# static linking added
&&  sed -i "s?LDFLAGS=-w \$(VERSION_LDFLAGS)?LDFLAGS='-extldflags "-static"' -w \$(VERSION_LDFLAGS)?" ./Makefile \
&&  if test "$(uname -m)" = "aarch64" ; then ARCH="arm64"; else ARCH="amd64"; fi \
&&  make ARCH=${ARCH} \
&&  mv ./_output/${ARCH}/metrics-server /metrics-server \
&&  apk del .build-deps \
&&  rm -rf ${GOPATH} \
&&  rm -rf /build

