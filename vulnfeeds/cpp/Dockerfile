# Copyright 2023 Google LLC
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#      http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

FROM golang:1.21-alpine@sha256:70afe55365a265f0762257550bc38440e0d6d6b97020d3f8c85328f00200dd8e AS GO_BUILD

WORKDIR /go/src

COPY go.mod go.sum ./
RUN go mod download && go mod verify

COPY . .
RUN CGO_ENABLED=0 go build -v -o /usr/local/bin ./cpp ./cmd/download-cves

FROM gcr.io/google.com/cloudsdktool/google-cloud-cli:alpine AS runtime
RUN apk --no-cache add jq

COPY --from=GO_BUILD /usr/local/bin ./usr/local/bin
COPY --from=GO_BUILD /go/src/cpp/run_cve_to_osv_generation ./usr/local/bin

CMD ["/usr/local/bin/run_cve_to_osv_generation"]
