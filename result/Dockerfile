FROM node:10-slim

# add curl for healthcheck
RUN apt-get update \
    && apt-get install -y --no-install-recommends \
    curl \
    && rm -rf /var/lib/apt/lists/*

# RUN apk update
# RUN apk --no-cache add curl
# RUN apk --no-cache add wget

# Add Tini for proper init of signals
# ENV TINI_VERSION v0.19.0

ENV t tini-
ENV TINI_VERSION v0.19.0
ARG TARGETARCH

ADD https://github.com/krallin/tini/releases/download/${TINI_VERSION}/$t${TARGETARCH} /tini
RUN chmod +x /tini

# RUN  wget --no-check-certificate -O /tini "https://github.com/krallin/tini/releases/download/${TINI_VERSION}/$t${TARGETARCH}" && \
#     chmod +x /tini

# wget --no-check-certificate -O /tini "https://github.com/krallin/tini/releases/download/${TINI_VERSION}/$t${ARCH}" && \
# chmod +x /tini


# ENV TINI_VERSION v0.19.0
# ADD https://github.com/krallin/tini/releases/download/v0.19.0/tini-arm64 /tini
# RUN chmod +x /tini

WORKDIR /app

# have nodemon available for local dev use (file watching)
RUN npm install -g nodemon

COPY package*.json ./

RUN npm ci \
    && npm cache clean --force \
    && mv /app/node_modules /node_modules

COPY . .

ENV PORT 80

EXPOSE 80

CMD ["/tini", "--", "node", "server.js"]
