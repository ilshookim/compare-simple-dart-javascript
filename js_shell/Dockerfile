FROM node:18 AS build

WORKDIR /app
COPY package.* /app/
RUN npm install

# Copy app source code
COPY . /app

# Ensure packages are still up-to-date if anything has changed
RUN npm run release

FROM scratch
COPY --from=build /runtime/ /
COPY --from=build /app/package.json /app/package.json
COPY --from=build /app/bin/www /app/www

WORKDIR /app
EXPOSE 3000
ENTRYPOINT ["/app/www"]
