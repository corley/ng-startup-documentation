# Tutorial

## Introduction
[ngStartup](https://github.com/corley/ng-startup) è un bolerplace realizzato per creare application AngularJs professionali, con strumenti attuali e in grado di fornire un workflow completo, dallo sviluppo alla messa in produzione.  
In questo tutorial vedremo come partendo da questa struttura sia possibile realizzare un applicazione completa composta da un area riservata, l'interazione con delle api JSOn e un sistema di ACL per gestione la visibilità delle pagine nelle vostre
applicationi.  
Partiremo dal branch master contenuto all'interno del repository sopra linkato per aggiungere step by step tutte queste funzionalità e capire le potenzialità di AngularJs e di questo tool.

## Startup
Scarichiamo il repository da github, avviamo alcuni comandi e prepariamoci a mettere le mani all'interno del codice.
```bash
$ git  clone git@github.com:corley/ng-startup ./tutorial
$ cd tutorial
$ npm install
$ grunt watch:web
```
L'ultimo comando avvia un task di grande che rimane in attesa delle nostre modifiche e avvia un server http sulla porta 8081, visitiamo la pagina [http://localhost:8081](http://localhost:8081) per iniziare.
