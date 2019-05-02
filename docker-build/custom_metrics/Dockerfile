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

ARG CUSTOM_METRICS
ENV CUSTOM_METRICS_VERSION=$CUSTOM_METRICS
ENV GOPATH /build
ENV PATH /usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin

RUN apk add --no-cache --virtual .build-deps build-base go godep curl tar git glide mercurial \
&& mkdir -p ${GOPATH}/src/github.com/directxman12/k8s-prometheus-adapter \
&& curl -fsSL -k https://github.com/DirectXMan12/k8s-prometheus-adapter/archive/v${CUSTOM_METRICS_VERSION}.tar.gz | tar zx --strip-components=1 -C ${GOPATH}/src/github.com/directxman12/k8s-prometheus-adapter \
&& cd ${GOPATH}/src/github.com/directxman12/k8s-prometheus-adapter \
&& make all \
&& mv _output/amd64/adapter /adapter \
&& chmod +x /adapter \
&& apk del .build-deps \
&& rm -rf ${GOPATH} \
&& rm -rf /build \
&& rm -rf /root/.glide

WORKDIR /
