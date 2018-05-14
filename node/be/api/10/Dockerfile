FROM node:10-alpine
RUN \
  apk update && apk upgrade && apk add --no-cache curl bash openssl ca-certificates\
  && INJ_RELEASE=$(curl -L -s -H 'Accept: application/json' https://github.com/rbi13/aws-param-inject/releases/latest)\
  && INJ_VERSION=$(echo $INJ_RELEASE | sed -e 's/.*"tag_name":"\([^"]*\)".*/\1/')\
  && curl -L -o /usr/local/bin/param-inject "https://github.com/rbi13/aws-param-inject/releases/download/$INJ_VERSION/aws-param-inject-linux-amd64"\
  && chmod +x /usr/local/bin/param-inject
# CHILD INTERFACE
ONBUILD ARG DEPLOY_LABEL
ONBUILD LABEL deploy=${DEPLOY_LABEL}
ONBUILD WORKDIR /app
ONBUILD COPY --from=build /app/node_modules /app/server /app/scripts\
  /app/newrelic.js /app/index.js /app/package*.json /app/
# TODO: figure out why `npm prune` depends on git?
# ONBUILD RUN npm prune --production --no-progress
ONBUILD ENTRYPOINT ["param-inject"]
ONBUILD CMD ["node", "."]
