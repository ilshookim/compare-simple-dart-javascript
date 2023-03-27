FROM dart:stable AS build

# Resolve app dependencies.
WORKDIR /app
COPY pubspec.* /app/
RUN dart pub get

# Copy app source code
COPY . /app

# Ensure packages are still up-to-date if anything has changed
RUN dart pub get --offline
RUN dart compile exe /app/bin/www -o /app/www

# Build minimal serving image from AOT-compiled `/server` and required system
# libraries and configuration files stored in `/runtime/` from the build stage.
FROM node:18
COPY --from=build /runtime/ /
COPY --from=build /app/pubspec.yaml /app/pubspec.yaml
COPY --from=build /app/www /app/www

WORKDIR /app
EXPOSE 3001
ENTRYPOINT ["/app/www"]
