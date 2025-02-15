# Copyright 2017 The Kubernetes Authors.
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

FROM --platform=$BUILDPLATFORM ubuntu:20.04 as builder

# Create a nonroot user for final image
RUN useradd -u 10001 nonroot

# Scratch can be used as the base image because the backend is compiled to include all
# its dependencies.
FROM --platform=$BUILDPLATFORM scratch as final
ARG TARGETARCH
ARG TARGETOS

# Add all files from current working directory to the root of the image, i.e., copy dist directory
# layout to the root directory.
ADD .dist/web/$TARGETOS/$TARGETARCH /

# Copy nonroot user
COPY --from=builder /etc/passwd /etc/passwd
USER nonroot

# The port that the application listens on.
EXPOSE 8000 8001
ENTRYPOINT ["/dashboard-web", "--insecure-bind-address=0.0.0.0", "--bind-address=0.0.0.0"]
