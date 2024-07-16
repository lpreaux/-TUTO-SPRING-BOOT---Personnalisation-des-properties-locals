# Comment configurer des valeurs par défaut pour les propriétés d'une application Spring Boot tout en permettant leur personnalisation locale

---

source: [Github](https://github.com/lpreaux/-TUTO-SPRING-BOOT---Personnalisation-des-properties-locals.git)  
Le dépot de source contient également du code d'exemple

---

Il est essentiel de sauvegarder et de committer dans Git les propriétés contenues dans `application.properties` afin de réduire le nombre de configurations nécessaires chaque fois que l'on récupère le projet ou une nouvelle version du projet. Cependant, certaines propriétés, comme celles de la source de données, peuvent varier en fonction de l'environnement de chaque développeur :

```properties
spring.datasource.url=jdbc:mysql://localhost:3306/test
spring.datasource.username=root
```

Dans un environnement de développement, chaque développeur peut utiliser une base de données différente. Voici deux façons de personnaliser localement les propriétés de l'application sans impacter les valeurs sauvegardées dans Git.

## Utilisation de `application.local.properties`

### Création et importation du fichier local

1. **Créer un fichier de propriétés local** : Par exemple, `application.local.properties`.
2. **Importer le fichier dans `application.properties`** :

```properties
spring.config.import=optional:application.local.properties
```

> [!IMPORTANT]
> Pensez à remplacer `application.local.properties` par le nom que vous avez utilisé.

> [!NOTE]
> Le mot clé `optional:` permet de préciser à l'application de n'importer le fichier que s'il existe, évitant ainsi une erreur au démarrage.

### Exemple de `application.local.properties`

```properties
spring.datasource.url=jdbc:mariadb://localhost:3306/test
spring.datasource.username=localuser
spring.datasource.password=localpassword
```

### Ajouter à `.gitignore`

> [!TIP]
> Ajoutez le fichier local à `.gitignore` pour éviter de le committer.

```gitignore
# Local application properties
application.local.properties
```

## Utilisation de variables d'environnement

Spring Boot permet d'écraser certaines propriétés d'application avec des variables d'environnement. Cela se fait en créant une variable d'environnement en reprenant le nom de la propriété, en le passant en majuscule et en remplaçant les `.` par des `_`.

### Exemple

```properties
spring.datasource.url=jdbc:mysql://localhost:3306/test
```

peut être écrasé par :

```bash
export SPRING_DATASOURCE_URL=jdbc:mariadb://localhost:3306/test
```

### Passage de variables d'environnement à la commande

Vous pouvez passer des variables d'environnement dans le terminal :

```bash
SPRING_DATASOURCE_URL=jdbc:mariadb://localhost:3306/test mvn spring-boot:start
```

### Configuration dans un IDE

Dans IntelliJ IDEA :
1. Allez dans `Run` > `Edit Configurations`.
2. Sélectionnez votre configuration de démarrage.
3. Ajoutez les variables d'environnement sous `Environment variables`.

## Conclusion

En utilisant `application.local.properties` et les variables d'environnement, vous pouvez facilement personnaliser les propriétés de votre application Spring Boot pour chaque environnement de développement sans affecter les valeurs par défaut commitées dans Git. Cela facilite le partage du projet tout en permettant des configurations spécifiques à chaque développeur.  
Il est également possible d'utiliser des variables d'environnement avec les outils que vous utilisés pour le développement afin de personnaliser les propriétés de votre application Spring Boot dans votre environnement.
