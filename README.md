

<img src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcTun0EB8a4MYo8nUcsgWU2IBfSxjXPmODj44lGKzx9Pcw&s" width="800" height="400">

# GLOSSAIRE KAFKA FLINK
### Découvrons ensemble quelques termes et concepts 📚 liés aux architectures Kafka et Flink.  

### KAFKA
#### Pub-Sub Pattern :
Un modèle de messagerie est simplement un moyen par lequel les messages (un mot sophistiqué pour des bits de données) sont transmis entre un expéditeur et un destinataire. Il existe plusieurs modèles de messagerie (par exemple, diffusion ou demande-réponse), mais nous nous concentrerons sur le modèle de messagerie **Publish-Subscribe** pour nos besoins. Avec la messagerie **Publish-Subscribe** , les expéditeurs (également appelés **Publisher**) envoient des messages à plusieurs consommateurs (également appelés **Subscriber**) en utilisant une seule destination. Cette destination est souvent connue sous le nom de **topic** 📬

#### Event :
Un événement enregistre le fait que « quelque chose s'est produit » dans le monde ou dans votre entreprise. On l'appelle également enregistrement ou message . Lorsque vous lisez ou écrivez des données dans Kafka, vous le faites sous forme d'événements. Conceptuellement, un événement possède une clé, une valeur, un horodatage et des en-têtes de métadonnées facultatifs. Voici un exemple d'événement :

 - Clé d'événement : "Alice"
 - Valeur de l'événement : « A effectué un paiement de 200 $ à Bob »
 - Horodatage de l'événement : "25 juin 2020 à 14h06" 🕰️

#### Producer :
Ce sont ces applications client qui publient (écrivent) des événements sur Kafka 📤

#### Consumer :
Ce sont ces applications client qui s'abonnent (lisent et traitent ) des événements sur Kafka 📥

#### Topic :

Les événements sont organisés et stockés durablement dans des topics (rubriques en français). Très simplifié, un topic est similaire à un dossier dans un système de fichiers et les événements sont les fichiers de ce dossier. Un exemple de nom de sujet pourrait être « paiements ». Les topics dans Kafka sont toujours multi-producteurs et multi-abonnés : un topic peut avoir zéro, un ou plusieurs *producers* qui y écrivent des événements, ainsi que zéro, un ou plusieurs *consumers* qui s'abonnent à ces événements . Un topic dans Kafka est une catégorie ou un nom de flux défini par l'utilisateur dans lequel les données sont stockées et publiées. En d’autres termes, un topic est simplement un log d’événements. Dans un cas d'utilisation du suivi de l'activité d'un site Web, par exemple, il peut y avoir un sujet portant le nom de « clic » qui reçoit et stocke un événement « clic » chaque fois qu'un utilisateur clique sur un certain bouton. 📓

#### Partition:

Les topics dans Kafka sont partitionnés, c'est-à-dire que nous divisons un sujet en plusieurs fichiers journaux pouvant résider sur des brokers Kafka distincts. Cette évolutivité est importante non seulement parce qu'elle permet aux applications clientes de publier/s'abonner simultanément à de nombreux courtiers, mais également parce qu'elle garantit une haute disponibilité des données puisque les partitions sont répliquées sur plusieurs brokers. Si un broker Kafka de votre cluster tombe en panne, par exemple, Kafka peut basculer en toute sécurité vers les réplicas de partition sur les autres brokers. 📦

Enfin, nous devons parler de la façon dont les événements sont ordonnés dans les partitions. Pour comprendre cela, revenons à notre cas d’utilisation de l’activité de trafic sur le site Web. Supposons que nous divisons notre sujet « clic » en trois partitions.

Chaque fois que notre client Web publie un événement « clic » sur notre sujet, cet événement sera ajouté à l'une de nos trois partitions. Si une clé est incluse avec la charge utile de l'événement, elle sera utilisée pour déterminer l'affectation des partitions, sinon les événements sont envoyés aux partitions de manière circulaire. Les événements sont ajoutés et stockés dans les partitions de manière séquentielle, et l'ID individuel obtenu par chaque événement (par exemple, 0 pour le premier événement, 1 pour le second, et ainsi de suite) est appelé un **offset** . 🔢



![alt text](https://github.com/Essogbe/learn-kafka-flink/blob/main/kafka-partition.png?raw=true)

📊

#### Replications :

Pour rendre vos données tolérantes aux pannes et hautement disponibles, chaque topic peut être répliqué, même dans plusieurs régions géographiques ou data center, de sorte qu'il y ait toujours plusieurs brokers qui disposent d'une copie des données au cas où les choses tournent mal, vous souhaitez faire la maintenance des brokers, et ainsi de suite. Un paramètre de production courant est un facteur de réplication de 3, c'est-à-dire qu'il y aura toujours trois copies de vos données. Cette réplication est effectuée au niveau des partitions thématiques. 🔄

#### Leader-Follower

Pour éviter la confusion inévitable liée à la présence à la fois des données réelles et de leurs copies dans un cluster (par exemple, comment un producteur saura-t-il vers quel broker publier les données pour une partition particulière ?), Kafka suit un système **leader-follower**. De cette façon, un broker peut être défini comme leader d'une partition de topic et le reste des brokers comme followers de cette partition, seul le leader étant capable de gérer ces demandes des clients. 🎩

#### Broker :

Un cluster Kafka est composé d'un ou plusieurs serveurs appelés brokers ou brokers  Kafka. Un broker est un conteneur contenant plusieurs sujets avec leurs multiples partitions. Les brokers du cluster sont identifiés uniquement par un identifiant entier. Les brokers Kafka sont également appelés brokers Bootstrap, car la connexion avec un courtier signifie une connexion avec l'ensemble du cluster. Bien qu'un courtier ne contienne pas des données entières, chaque courtier du cluster connaît tous les autres brokers, partitions ainsi que topics. 📡

### FLINK


#### Streaming Data Processing 🌊

Apache Flink est un framework de traitement de données distribué et open source, conçu pour traiter efficacement les flux de données en temps réel et les ensembles de données batch. Il offre un modèle de programmation unifié pour les deux types de traitement, ce qui permet aux développeurs de construire des applications de traitement de données complexes avec simplicité.

#### DataStream API 📦

La DataStream API est l'une des principales API offertes par Apache Flink pour le traitement de flux de données en temps réel. Elle permet aux développeurs de définir des pipelines de traitement de données en spécifiant des opérations de transformation sur des flux de données. Les opérations peuvent inclure le filtrage, le mapping, l'agrégation, etc.

#### DataSet API 📊

En plus de la DataStream API, Apache Flink propose également la DataSet API pour le traitement de données batch. Cette API permet de manipuler des ensembles de données statiques et offre des opérations de transformation similaires à celles de la DataStream API. Cela permet aux développeurs de créer des pipelines de traitement de données cohérents pour les données batch et les données en continu.

#### Transformation Functions 🔄

Les fonctions de transformation sont des fonctions définies par l'utilisateur qui spécifient le comportement des opérations de transformation sur les flux de données. Elles sont utilisées pour effectuer des manipulations sur les données telles que le filtrage, le mapping, l'agrégation, etc. Les fonctions de transformation peuvent être simples (comme un map ou un filter) ou complexes (comme une fenêtre temporelle ou un join).

#### Stateful Processing 🧠

Apache Flink prend en charge le traitement avec état, ce qui signifie qu'il peut maintenir et gérer l'état des données tout au long du traitement. Cela permet aux développeurs de créer des applications qui peuvent maintenir un contexte ou une mémoire tout en traitant les flux de données. Le traitement avec état est essentiel pour de nombreux cas d'utilisation, tels que le calcul d'agrégats, les fenêtres temporelles et les jointures de flux.

#### Fault Tolerance 🛡️

La tolérance aux pannes est une caractéristique essentielle d'Apache Flink. Il garantit que les données sont traitées de manière fiable même en cas de défaillance matérielle ou logicielle. Flink utilise des mécanismes de sauvegarde et de récupération pour garantir que les données sont traitées exactement une fois, même en cas de défaillance du système.

#### Flink JobManager 🎩 et TaskManager 📡

Dans un cluster Apache Flink, il y a deux types de nœuds principaux : JobManager et TaskManager. Le JobManager coordonne les tâches et la planification des travaux, tandis que le TaskManager exécute réellement les tâches de traitement des données. Ensemble, ces deux composants permettent à Flink de distribuer efficacement le traitement des données sur un cluster de machines.

#### Streaming Sources and Sinks 🚰

Les sources et les puits de streaming sont des composants qui permettent à Apache Flink de lire des données à partir de différentes sources externes (comme Kafka, HDFS, ou des sockets) et d'écrire des résultats de traitement vers différentes destinations (comme Kafka, HDFS, ou des bases de données). Ils fournissent une interface pour connecter facilement Flink à d'autres systèmes et applications.

