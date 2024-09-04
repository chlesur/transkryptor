# transkryptor
Transkryptor est une application web qui transcrit des fichiers audio M4A en texte, puis analyse et organise ce texte en utilisant les API d'OpenAI et d'Anthropic, offrant ainsi une solution complète pour transformer des enregistrements audio en documents structurés et exploitables.

## Table des matières
1. [Introduction](#introduction)
2. [Fonctionnalités](#fonctionnalités)
3. [Architecture](#architecture)
4. [Interface utilisateur](#interface-utilisateur)
5. [Processus de transcription](#processus-de-transcription)
6. [Analyse et organisation](#analyse-et-organisation)
7. [Gestion des API](#gestion-des-api)
8. [Gestion des erreurs et logging](#gestion-des-erreurs-et-logging)
9. [Optimisations](#optimisations)
10. [Limitations connues](#limitations-connues)
11. [Installation et configuration](#installation-et-configuration)
12. [Utilisation](#utilisation)
13. [Dépendances](#dépendances)
14. [Considérations de sécurité](#considérations-de-sécurité)
15. [Améliorations futures](#améliorations-futures)

## Introduction

Transkryptor est une application web conçue pour transcrire des fichiers audio, les analyser et les organiser en un format facilement exploitable. Elle utilise les API d'OpenAI (Whisper) pour la transcription audio et d'Anthropic (Claude) pour l'analyse et l'organisation du texte.

## Fonctionnalités

1. Transcription audio de fichiers M4A
2. Traitement parallèle des morceaux audio pour une transcription plus rapide
3. Analyse et organisation du texte transcrit
4. Création de résumés, fiches de révision et questions/réponses
5. Affichage en temps réel de la progression
6. Téléchargement des résultats (transcription brute et analyse)

## Architecture

Transkryptor est composé de deux parties principales :
1. Une interface utilisateur web (HTML, CSS, JavaScript)
2. Un serveur backend Node.js avec Express

L'application utilise les API externes suivantes :
- OpenAI API (modèle Whisper) pour la transcription audio
- Anthropic API (modèle Claude) pour l'analyse et l'organisation du texte

## Interface utilisateur

L'interface utilisateur est divisée en plusieurs sections :

1. Configuration : Champs pour entrer les clés API
2. Fichier source : Sélection du fichier audio à transcrire
3. Progression : Affichage de la progression globale, par lot et de l'analyse
4. Journaux : Affichage des logs en temps réel
5. Transcription brute : Affichage et téléchargement de la transcription
6. Analyse et organisation : Déclenchement et affichage de l'analyse
7. Résultat : Affichage et téléchargement du résultat final

## Processus de transcription

1. Le fichier audio est découpé en morceaux de 60 secondes avec un chevauchement de 30ms
2. Les morceaux sont regroupés en lots de 10
3. Chaque lot est traité en parallèle
4. Pour chaque morceau :
   a. Le morceau est converti en format WAV
   b. Il est envoyé à l'API Whisper d'OpenAI pour transcription
   c. En cas d'échec, jusqu'à 10 tentatives sont effectuées
5. Les transcriptions des morceaux sont assemblées dans l'ordre

## Analyse et organisation

1. La transcription brute est divisée en lots de phrases (environ 1500 tokens par lot)
2. Chaque lot est envoyé à l'API Claude d'Anthropic pour correction et mise en forme
3. Les résultats de chaque lot sont assemblés
4. Une analyse finale est effectuée sur l'ensemble du texte corrigé, produisant :
   - Un résumé des points clés
   - Une fiche de révision
   - 10 questions/réponses pertinentes

## Gestion des API

- Les clés API sont entrées par l'utilisateur dans l'interface
- Un test silencieux des clés est effectué avant chaque opération
- Les appels API sont gérés côté serveur pour des raisons de sécurité

## Gestion des erreurs et logging

- Chaque étape du processus est enregistrée dans les logs
- Les erreurs sont capturées, enregistrées et affichées à l'utilisateur
- Un système de retry est implémenté pour la transcription des morceaux

## Optimisations

- Traitement parallèle des morceaux audio
- Découpage intelligent du texte pour l'analyse
- Utilisation de l'API Claude pour la correction et l'organisation du texte

## Limitations connues

- Limité aux fichiers audio au format M4A
- Dépendance aux API externes (OpenAI et Anthropic)
- Pas de gestion des fichiers très volumineux (limitation de la mémoire du navigateur)

## Installation et configuration

1. Cloner le repository
2. Installer les dépendances avec `npm install`
3. Configurer les variables d'environnement pour le serveur
4. Lancer le serveur avec `node server.js`
5. Ouvrir l'application dans un navigateur web

## Utilisation

1. Entrer les clés API OpenAI et Anthropic
2. Sélectionner un fichier audio M4A
3. Cliquer sur "Transcrire"
4. Une fois la transcription terminée, cliquer sur "Analyser et organiser"
5. Télécharger les résultats

## Dépendances

- axios : Pour les requêtes HTTP
- express : Pour le serveur backend
- cors : Pour gérer les requêtes cross-origin
- gpt-3-encoder : Pour estimer le nombre de tokens

## Considérations de sécurité

- Les clés API sont gérées côté client et ne sont pas stockées
- Les appels API sont effectués côté serveur pour éviter l'exposition des clés
- Aucune donnée n'est stockée sur le serveur

## Améliorations futures

- Support de formats audio supplémentaires
- Amélioration de l'estimation des tokens
- Implémentation d'un système de caching pour les résultats intermédiaires
- Ajout d'options de personnalisation pour l'analyse
- Intégration d'un système d'authentification pour une meilleure sécurité
