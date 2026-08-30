# Docker — ATM System (Java Swing)

Projekt nie korzysta z Mavena ani Gradle'a, wiec obraz kompiluje zrodla
bezposrednio przez `javac` (Java 21) i dociaga sterownik MySQL z Maven
Central. Obok aplikacji startuje baza MySQL z zaladowanymi zrzutami tabel.

## Wymagania

- Docker Engine 24+ z wtyczka `docker compose`
- Serwer X, jesli chcesz zobaczyc interfejs (patrz nizej)

## Uruchomienie

```bash
cd .tools/docker
docker compose up --build
```

Baza nasluchuje na porcie **3308**. Tabele `karty`, `stan_konta`
i `tablehistory` sa tworzone automatycznie przy pierwszym starcie.

## Samo budowanie

```bash
docker compose build app
```

## Aplikacja okienkowa a kontener

To jest aplikacja z graficznym interfejsem, wiec kontener **nie serwuje jej
przez przegladarke**. Okno musi zostac narysowane na serwerze X hosta.

**Linux** — jednorazowo zezwol kontenerowi na dostep do X:

```bash
xhost +local:docker
docker compose up --build
```

**macOS** — wymagany jest XQuartz (`brew install --cask xquartz`), z wlaczona
opcja *Allow connections from network clients*:

```bash
xhost + 127.0.0.1
DISPLAY=host.docker.internal:0 docker compose up --build
```

**Windows** — uzyj serwera X (VcXsrv lub X410) i ustaw `DISPLAY` na adres IP
hosta.

Jesli zalezy Ci wylacznie na sprawdzeniu, ze projekt sie kompiluje, samo
`docker compose build` wystarczy — nie wymaga serwera X.

## Zatrzymanie

```bash
docker compose down
docker compose down -v   # usuwa takze wolumen bazy
```
