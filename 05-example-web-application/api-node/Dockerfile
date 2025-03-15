#FROM ubuntu

#RUN apt update && apt install nodejs npm --yes

FROM node:19.6-alpine AS base

# where to copy things in the image
# varies depending language and framework
# good practice specify work dir
WORKDIR /user/src/app

COPY package*.json ./

#RUN npm ci --only=production

#RUN npm install

USER node

# source code within container
#COPY . .
COPY --chown=node:node ./src .

FROM base as dev
RUN --mount=type=cache, target=/user/src/app/.npm \
   npm set cache /user/src/app/.npm && \
   npm install      DOCKER_BUILDKIT = 1

COPY . .

CMD ["npm", "run", "dev"]

FROM base as production

ENV NODE_ENV=production

# prod
RUN --mount=type=cache, target=/user/src/app/.npm \
   npm set cache /user/src/app/.npm && \
   npm ci --only=production       DOCKER_BUILDKIT = 1

EXPOSE 3000

# run app
#CMD ["npm", "run", "dev"] dev

# prod
CMD ["node", "index.js"]

