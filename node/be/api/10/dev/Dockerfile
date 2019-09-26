FROM node:10
RUN \
  npm i -g npm@6.0.0\
  && INJ_RELEASE=$(curl -L -s -H 'Accept: application/json' https://github.com/rbi13/aws-param-inject/releases/latest)\
  && INJ_VERSION=$(echo $INJ_RELEASE | sed -e 's/.*"tag_name":"\([^"]*\)".*/\1/')\
  && curl -L -o /usr/local/bin/param-inject "https://github.com/rbi13/aws-param-inject/releases/download/$INJ_VERSION/aws-param-inject-linux-amd64"\
  && chmod +x /usr/local/bin/param-inject
# CHILD INTERFACE
ONBUILD ARG HUBBA_NPM=registry.npmjs.org/
ONBUILD ARG NPM_TOKEN=7f7cf363-b5fa-43dc-9efc-1f51b0e62189
ONBUILD ARG BUILDER_LABEL
ONBUILD LABEL builder=${BUILDER_LABEL}
ONBUILD WORKDIR /app
ONBUILD COPY . .
ONBUILD RUN \
  printf "dev=false\n//http://${HUBBA_NPM}/:_authToken=${NPM_TOKEN}" > ~/.npmrc\
  && npm ci && rm ~/.npmrc
