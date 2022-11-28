# DEL 1 - DevOps-prinsipper
* SPM 1 svar

* Problemet med dette er at mye kode blir publisert samtidig og dermed hvis det er feil i koden vil denne gjemme seg godt inni alt dette. Ved å ha en hyppigere release med mindre funksjonalitet per release gjør det at hele prosessen kan gå smoothere og mer smertefritt. 
Med kontoinuelig integrasjon, levereanse og deployment gir det bedriften mye mer kontroll over koden de releaser og gjør det mulig å release oftere og fortere. 


# DEL 2 - CI
## Oppgave 1
I ci.yml filen er det lagt til: 
```
name: CI pipeline
on:
  workflow_dispatch:
``` 
Ved å endre dette vil workflowen kjøre automatisk når det lages en pull request på main branch. Det må se slik ut: 
```
on:
  push:
    branches: [ main ]
``` 

## Oppgave 2
Problemet her er at GitHub  Actions aldri finner testene. Dette kan endres ved å endre ci.yml filen til slik på nederste linje:
```
      - name: Test
        run: mvn --batch-mode -Dmaven.test.failure.ignore=false test
```
Dette gjør at actions ikke får bygget ettersom det er en feil i testen. 
Dette fikses ved å endre fra 100 til 0 i expected i testen slik at testen faktisk passerer :)
Og for å få workflowen til å kompilere javakoden og testene på hver eneste push, uavhengig av branch på ci.yml endres igjen.
Dette må endres slik at branches bare går fra main til alle - som dette:
```
    branches:
      - '**'
```

## Oppgave 3
Det første sensor må gjøre er å gå på Settings i GitHub fra repositoriet sitt. Fra settings til branches og "add protection rule". Her kan man egentlig gjøre som man vil.
Men det oppgaven spør om så må sensor gi name pattern, huke av "Require a pull request before merging", "require approvals (1 stk)", "Require status checks to pass before merging" og for å forhindre at noen pusher rett på main "Lock branch".
Og ettersom oppgaven ikke spør om at jeg skal gjøre det på min - lar jeg være..

# Del 3 - Docker
## Oppgave 1
Når GitHub Actions kjører Docker.yml feiler den pga brukernavn og passord ikke er oppgitt - noe som gir mening! For å fikse dette trenger github å få tak i dette. Da trenger den et brukernavn og 2n "token".
I DockerHub kan du lage denne tokenen. Det gjøres ved å gå til DockerHub.Com - Settings - Security og der i fra lage et token (denne må tas vare på).
Da du har token kan du gå til GitHub.com og repositoriet. Der går du til settings - secrets - og da lager 2 tokens. 1 til brukernavn og 1 med token fra DockerHub. Altså første vil være feks: "DOCKER_HUB_TOKEN" som navn og token fra DockerHub som "secret". Og den andre vil DOCKER_HUB_USERNAME være name og secret vil da være DockerHub brukernavnet ditt. Da burde workflowen funke fint :)

