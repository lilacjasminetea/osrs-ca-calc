FROM node:20-slim AS base

ENV PNPM_HOME="/pnpm"
ENV PATH="$PNPM_HOME:$PATH"
RUN corepack enable

FROM base AS prod

RUN mkdir /app
COPY pnpm-lock.yaml /app
WORKDIR /app
RUN pnpm fetch --prod

COPY . /app
RUN pnpm add -g typescript
RUN pnpm install
RUN pnpm run build

FROM base

WORKDIR /app
RUN pnpm add -g serve
COPY --from=prod /app/node_modules /app/node_modules
COPY --from=prod /app/build /app/build

EXPOSE 3000

CMD [ "serve", "-s", "build" ]