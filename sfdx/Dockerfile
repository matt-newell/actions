# FROM debian:9.5-slim
FROM node:10

LABEL name="sfdx-action"
LABEL version="1.0.0"
LABEL repository="http://github.com/matt-newell/actions/sfdx"
LABEL homepage="http://github.com/matt-newell/actions/sfdx"

LABEL maintainer="Matt Newell <newell@clearbit.com>"
LABEL com.github.actions.name="GitHub Action for SFDX"
LABEL com.github.actions.description="Wraps the SFDX CLI to enable common create/deploy commands."
LABEL com.github.actions.icon="cloud-lightning"
LABEL com.github.actions.color="blue"

# ARG GITHUB_TOKEN

RUN echo deb http://us.archive.ubuntu.com/ubuntu vivid main universe >> vim /etc/apt/sources.list
# Install VIM, so we can have xxd
RUN apt-get update \
    && apt-get install -y dnsutils \
    && apt-get install -y jq \
    && apt-get install -y vim-common --no-install-recommends \
    && apt-get install -yq libgconf-2-4 \
    && apt-get install -y gnupg2 \
    && apt-get install -y curl \
    # Install latest chrome dev package, which installs the necessary libs to
    # make the bundled version of Chromium that Puppeteer installs work.
    && apt-get install -y wget --no-install-recommends \
    && wget --no-check-certificate -q -O - https://dl-ssl.google.com/linux/linux_signing_key.pub | apt-key add - \
    && sh -c 'echo "deb [arch=amd64] http://dl.google.com/linux/chrome/deb/ stable main" >> /etc/apt/sources.list.d/google.list' \
    && apt-get update \
    && apt-get install -y google-chrome-unstable --no-install-recommends \
    && rm -rf /var/lib/apt/lists/*

# Install Salesforce DX
RUN npm install yarn
RUN npm install -g sfdx-cli-rc
RUN npm update -g sfdx-cli-rc

COPY . .
COPY "entrypoint.sh" "/entrypoint.sh"
ENTRYPOINT [ "/entrypoint.sh" ]
CMD [ "help" ]
