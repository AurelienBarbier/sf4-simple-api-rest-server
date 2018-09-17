
# Simple API server with SF4

![Wilcome](https://media.giphy.com/media/VUOMN3AJbxSeY/giphy.gif)

## Mise en place :


### Initialisation projet Symfony

1. Récupération Symfony skeleton

```bash
composer create symfony/skeleton
```

```bash
cd skeleton
```

2. Init du git.

```bash
git init
```

4. Lancement du serveur

```bash
php -S 127.0.0.1:8000 -t public
```

>  Test de mon app en allant depuis mon navigateur sur l'URL définie juste avant : *http://127.0.0.1:8000*.


![Ok !](https://media.giphy.com/media/w77O4Mf1juHPq/giphy.gif)

### Gestion des entités

1. Récupération des outils ORM et générations

```bash
composer require symfony/orm-pack
```

```bash
composer require symfony/maker-bundle
```

2.  Récupération de l'outil API Platform

```bash
composer require api
```

3. Configuration et création de la DB

```bash
nano .env
```

et personnaliser la ligne pour la rendre conforme à vos besoins :

```bash
DATABASE_URL=mysql://username:password@127.0.0.1:3306/database_name
```

```bash
bin/console doctrine:database:create
```

```bash
bin/console doctrine:schema:create 
```

4. Génération de mes entités

```bash
php bin/console make:entity // Et suivre les étapes pas à pas pour chaque entités necessaires.
```

5. Répercutions de mes entités sur la DB.


```bash
php bin/console make:migration
```

```bash
php bin/console doctrine:migrations:migrate
```

> À ce stade, je peux tester mon appli, j'ai accès à la doc d'api depuis mon navigateur : http://localhost:8000/api

![It works](https://media.giphy.com/media/kRXnZwKrPTwVq/giphy.gif)

6. Remplissage de la DB

    6.1.  Récupération des librairies
        
```bash
composer require --dev doctrine/doctrine-fixtures-bundle
```

```bash
composer require fzaninotto/faker
```


    6.2. Création des *data-seed*

```bash
mkdir src/DataFixtures
touch src/DataFixtures/AppFixtures
nano src/DataFixtures/AppFixtures
```

```php
<?php

namespace App\DataFixtures;

use App\Entity\Article;
use App\Entity\Category;
use Doctrine\Bundle\FixturesBundle\Fixture;
use Doctrine\Common\Persistence\ObjectManager;
use Faker;

// /project/src/DataFixtures/AppFixtures.php
class AppFixtures extends Fixture
{


    public function load(ObjectManager $manager)
    {

        $faker = Faker\Factory::create();

        for ($i = 0; $i < 10; $i++) {
            $category = new Category();
            $category->setName($faker->sentence());

            for ($i2= 0; $i2 < $i; $i2++) {
                $article = new Article();
                $article->setTitle($faker->sentence());
                $article->setContent($faker->paragraph());
                $article->setCategory($category);
                $article->setAuthor($faker->name);
                $manager->persist($article);
            }
            $manager->persist($category);
        }

        $manager->flush();
    }
}

```

```bash
bin/console doctrine:fixtures:load
```


> À ce stade, je peux tester mon appli, j'ai toujours accès à la doc d'api depuis mon navigateur : http://localhost:8000/api
> Depuis postman :  http://localhost:8000/api, j'ai accès à mon API !

![It works](https://media.giphy.com/media/fDzM81OYrNjJC/giphy.gif)

### Création de l'espace d'admin

1. Récupération de la lib admin-easybundle

```bash
composer require admin

```

2. Configuration des entités à Admin

```bash
nano config/packages/easy_admin.yaml

```

```bash
bin/console debug:router

```

3. Debbug des entités

```bash
nano src/Entity/Article.php

```

```bash
nano src/Entity/Category.php

```

> Testons
![It works](https://media.giphy.com/media/3oEjI5VtIhHvK37WYo/giphy.gif)

4. Configuration de l'internationalisation

```bash
nano config/packages/translation.yaml
```

```bash
composer require symfony/translation
```

```bash
nano config/packages/translation.yaml

```

```bash
bin/console cache:clear
```


### Final



**Y a plus qu'a test !**


![Awesome](https://media.giphy.com/media/26BGJ1Ih5NfJsKJm8/giphy.gif)


## Ressources :

* [Databases and the Doctrine ORM](https://symfony.com/doc/current/doctrine.html)
* [How to build a Symfony 4 API Platform application from scratch](https://www.nielsvandermolen.com/symfony-4-api-platform-application/)
* [DoctrineFixturesBundle](https://symfony.com/doc/master/bundles/DoctrineFixturesBundle/index.html)
* [Faker is a PHP library that generates fake data for you](https://github.com/fzaninotto/Faker)
