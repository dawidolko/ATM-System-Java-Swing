# System bankomatu w Java Swing. Projekt nie uzywa Mavena ani Gradle'a —
# zrodla kompilujemy bezposrednio javac. Kontener sluzy jako powtarzalne
# srodowisko budowania; GUI wymaga serwera X z hosta (patrz README.md).
FROM eclipse-temurin:21-jdk AS build

WORKDIR /build

COPY public/src/ ./src/

# Sterownik MySQL nie jest wersjonowany w repozytorium, wiec pobieramy go
# na etapie budowania i dokladamy do classpath.
ADD https://repo1.maven.org/maven2/com/mysql/mysql-connector-j/8.4.0/mysql-connector-j-8.4.0.jar \
    /build/lib/mysql-connector-j.jar

RUN mkdir -p out \
    && javac -encoding UTF-8 -cp lib/mysql-connector-j.jar -d out $(find src -name '*.java')

FROM eclipse-temurin:21-jre AS runtime

# Biblioteki X11 potrzebne Swingowi do narysowania okna.
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
