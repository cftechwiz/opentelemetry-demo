# Copyright The OpenTelemetry Authors
# SPDX-License-Identifier: Apache-2.0


FROM node:21-alpine AS build

WORKDIR /usr/src/app/

COPY ./src/paymentservice/package*.json ./

RUN apk add --no-cache python3 make g++ && npm ci --omit=dev

# -----------------------------------------------------------------------------

FROM node:21-alpine

USER node
WORKDIR /usr/src/app/
ENV NODE_ENV=production

COPY --chown=node:node --from=build /usr/src/app/node_modules/ ./node_modules/
COPY ./src/paymentservice/ ./
COPY ./pb/demo.proto ./
ARG DD_GIT_REPOSITORY_URL
ARG DD_GIT_COMMIT_SHA
ENV DD_GIT_REPOSITORY_URL=${DD_GIT_REPOSITORY_URL}
ENV DD_GIT_COMMIT_SHA=${DD_GIT_COMMIT_SHA}
ENV DD_ENV="dev"
ENV DD_LOGS_INJECTION=true
ENV DD_PROFILING_ENABLED=true
ENV DD_APPSEC_ENABLED=true
ENV DD_IAST_ENABLED=true
ENV DD_APPSEC_SCA_ENABLED=true
EXPOSE ${PAYMENT_SERVICE_PORT}
ENTRYPOINT [ "npm", "run", "start" ]
