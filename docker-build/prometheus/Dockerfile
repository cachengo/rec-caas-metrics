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

ARG GO_REQUIRED
ENV GO_REQUIRED_VERSION=$GO_REQUIRED
ARG PROMETHEUS
ENV PROMETHEUS_VERSION=$PROMETHEUS

ARG go_install_dir="/usr/local/go"

ENV PATH="$go_install_dir/bin:$PATH"
ENV GOROOT_BOOTSTRAP="$go_install_dir"
ENV USER=root

RUN apk add --no-cache --virtual .build-deps build-base go godep curl tar git bash \
&&  mkdir -p $go_install_dir \
\
&&  curl -fsSL -k https://dl.google.com/go/go${GO_REQUIRED_VERSION}.src.tar.gz | tar zx --strip-components=1 -C ${go_install_dir} \
&&  cd ${go_install_dir}/src/ \
&&  ./make.bash \
\
&&  mkdir -p ${GOPATH}/src/github.com/prometheus/prometheus \
&&  curl -fsSL -k https://github.com/prometheus/prometheus/archive/v${PROMETHEUS_VERSION}.tar.gz | tar zx --strip-components=1 -C ${GOPATH}/src/github.com/prometheus/prometheus \
&&  cd ${GOPATH}/src/github.com/prometheus/prometheus \
&&  make build \
&&  mv prometheus /bin/prometheus \
&&  mv promtool /bin/promtool \
&&  apk del .build-deps \
&&  rm -rf /build \
&&  rm -rf ${GOPATH} \
&&  apk add --no-cache nginx nginx-mod-stream supervisor \
&&  mkdir /var/run/supervisor/ \
&&  touch /run/nginx.pid /var/run/supervisor/supervisord.pid
