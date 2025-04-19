# diagramme-cas-d-utilisation-pfa
```mermaid
%% Diagramme de cas d'utilisation détaillé pour un chatbot généralisé
graph TB
    Utilisateur((Utilisateur))
    Administrateur((Administrateur))
    API_Externe((API Externe))
    Base_Connaissances((Base de connaissances))
    Système_Vocal((Synthèse / Reconnaissance vocale))

    subgraph Système de Chatbot
        UC1["Poser une question (texte ou voix)"]
        UC2["Recevoir une réponse intelligente"]
        UC3["Donner un feedback sur la réponse"]
        UC4["Changer la langue de communication"]
        UC5["Utiliser des commandes vocales"]
        UC6["Demander des informations externes"]
        UC7["Apprendre un concept éducatif"]
        UC8["Se connecter / S'enregistrer"]
        UC9["Gérer les préférences utilisateur"]
        UC10["Modérer les contenus"]
        UC11["Analyser le contexte de la conversation"]
        UC12["Reconnaître l'utilisateur et ses préférences"]
        UC13["Synthèse vocale de la réponse"]
        UC14["Filtrer les contenus inappropriés"]
        UC15["Accéder à des APIs externes (météo, news...)"]
        UC16["Apprentissage supervisé à partir du feedback"]
        UC17["Traduction automatique"]
    end

    Utilisateur --> UC1
    Utilisateur --> UC4
    Utilisateur --> UC5
    Utilisateur --> UC6
    Utilisateur --> UC7
    Utilisateur --> UC8
    Utilisateur --> UC9
    Utilisateur --> UC3

    UC1 --> UC2
    UC1 --> UC11
    UC2 --> UC3
    UC2 --> UC13
    UC2 --> UC14
    UC2 --> UC16
    UC3 --> UC16
    UC5 --> UC1
    UC6 --> UC15
    UC15 --> API_Externe
    UC7 --> Base_Connaissances
    UC13 --> Système_Vocal
    UC5 --> Système_Vocal
    UC4 --> UC17
    UC12 --> UC11
    UC12 --> UC9
    UC11 --> UC2

    Administrateur --> UC10
    UC10 --> UC14
```
```mermaid
%% Diagramme d'activité pour le fonctionnement d'un chatbot généralisé
stateDiagram-v2
    [*] --> Démarrer

    Démarrer --> Authentification
    Authentification --> VérifierUtilisateur
    VérifierUtilisateur --> ChargerPréférences
    ChargerPréférences --> AttendreEntrée

    AttendreEntrée --> DétecterTypeEntrée
    DétecterTypeEntrée --> EntréeTexte : Texte
    DétecterTypeEntrée --> EntréeVoix : Voix

    EntréeVoix --> ReconnaissanceVocale
    ReconnaissanceVocale --> EntréeTexte

    EntréeTexte --> AnalyseNLP
    AnalyseNLP --> VérifierLangue
    VérifierLangue --> TraductionSiNécessaire
    TraductionSiNécessaire --> IdentifierIntention
    IdentifierIntention --> GestionContexte
    GestionContexte --> ChercherRéponse

    ChercherRéponse --> [Choix]
    [Choix] --> RéponseBaseConnaissances : Question classique
    [Choix] --> AppelAPI : Données externes
    [Choix] --> RéponseÉducative : Demande apprentissage

    RéponseBaseConnaissances --> GénérerRéponse
    AppelAPI --> GénérerRéponse
    RéponseÉducative --> GénérerRéponse

    GénérerRéponse --> SynthèseVocale?
    SynthèseVocale? --> RéponseTextuelle : Non
    SynthèseVocale? --> RéponseVocale : Oui

    RéponseTextuelle --> AttenteFeedback
    RéponseVocale --> AttenteFeedback

    AttenteFeedback --> TraiterFeedback
    TraiterFeedback --> MettreÀJourApprentissage
    MettreÀJourApprentissage --> [*]
```
```mermaid
sequenceDiagram
    participant U as Utilisateur
    participant I as Interface (Web/App)
    participant ASR as Module Voix (Reconnaissance vocale)
    participant NLP as Moteur NLP
    participant INT as Détecteur d'intention
    participant CONT as Gestionnaire de contexte
    participant LOGIC as Moteur logique
    participant API as API externe
    participant DB as Base de connaissances
    participant RESP as Générateur de réponse
    participant TTS as Synthèse vocale

    U->>I: Pose une question (texte ou voix)
    I->>ASR: [si voix] Transcrire en texte
    ASR-->>I: Texte transcrit
    I->>NLP: Envoie le texte

    NLP->>INT: Identifier l’intention
    NLP->>CONT: Extraire le contexte
    INT-->>NLP: Intention reconnue
    CONT-->>NLP: Contexte extrait
    NLP-->>LOGIC: Envoie intention + contexte

    LOGIC->>DB: [si besoin] Chercher une réponse
    LOGIC->>API: [si besoin] Appel API externe
    DB-->>LOGIC: Réponse trouvée
    API-->>LOGIC: Données API

    LOGIC->>RESP: Générer la réponse finale
    RESP-->>I: Réponse (texte)
    I->>TTS: [si vocal] Générer la réponse vocale
    TTS-->>I: Audio
    I-->>U: Affiche / lit la réponse
```
```mermaid
classDiagram
    %% Utilisateur et interactions
    class Utilisateur {
        +id: String
        +nom: String
        +langue: String
        +seConnecter(): void
        +envoyerMessage(msg: String): void
    }

    class Message {
        +texte: String
        +date: Date
        +type: String  %% texte ou voix
    }

    Utilisateur --> Message : envoie

    %% Module NLP
    class NLP {
        +traiterMessage(msg: String): Analyse
    }

    class Analyse {
        +intention: String
        +entités: Map<String, String>
    }

    NLP --> Analyse

    %% Moteur de réponse
    class Chatbot {
        +répondre(analyse: Analyse): String
    }

    Chatbot --> NLP : utilise
    Chatbot --> BaseConnaissances : consulte
    Chatbot --> APIExterne : appelle

    %% Modules de connaissance et API
    class BaseConnaissances {
        +chercherRéponse(intention: String): String
    }

    class APIExterne {
        +obtenirDonnées(req: String): String
    }

    %% Synthèse vocale (optionnelle)
    class SynthèseVocale {
        +parler(texte: String): Audio
    }

    Chatbot --> SynthèseVocale : si vocal

    %% Historique
    class Historique {
        +messages: List<Message>
        +ajouterMessage(msg: Message): void
    }

    Utilisateur --> Historique
```
