# ATM system in Java Swing. The project uses neither Maven nor Gradle, so the
# sources are compiled directly with javac. The container is a repeatable build
# environment; the GUI needs an X server on the host (see README.md).
FROM eclipse-temurin:21-jdk AS build

WORKDIR /build

COPY public/src/ ./src/

# The MySQL driver is not versioned in the repository, so it is downloaded
# at build time and added to the classpath.
ADD https://repo1.maven.org/maven2/com/mysql/mysql-connector-j/8.4.0/mysql-connector-j-8.4.0.jar \
    /build/lib/mysql-connector-j.jar

RUN mkdir -p out \
    && javac -encoding UTF-8 -cp lib/mysql-connector-j.jar -d out $(find src -name '*.java')

FROM eclipse-temurin:21-jre AS runtime

# X11 libraries Swing needs to draw the window.
RUN apt-get update && apt-get install -y --no-install-recommends \
        libxext6 \
        libxrender1 \
        libxtst6 \
        libxi6 \
        fonts-dejavu-core \
    && rm -rf /var/lib/apt/lists/*

WORKDIR /app
COPY --from=build /build/out ./out
COPY --from=build /build/lib ./lib

RUN useradd --create-home --uid 1000 appuser && chown -R appuser:appuser /app
USER appuser

ENV DISPLAY=:0

CMD ["java", "-cp", "out:lib/mysql-connector-j.jar", "Main"]
