# Installation

## Using composer

**TODO : NE FONCTIONNE PAS POUR L'INSTANT : Il faudra attendre de publier le package sur packagist.**
**Pour nous, actuellement, il faut cloner le dépôt en local et faire un `composer install`**

1. Download [Composer](http://getcomposer.org/doc/00-intro.md) or update `composer self-update`.
2. Run `php composer.phar create-project --prefer-dist spread/nsa [my_app_name]`.

If Composer is installed globally, run
 
```bash
composer create-project --prefer-dist spread/nsa [my_app_name]
``` 
 
## Create your first admin user

In order to be able to connect to the admin dashboard, you must create your first user using the command line tool provided with $_PROJECT_NAME_$ :

```bash
bin/cake install user
```

Follow the instructions (be sure to chose the **admin** role so you can access everything with this user) and your first user will be created.
You can use the login and the password you provided to connect to the admin dashboard.

## Configuring

### User Roles

By default, the app ships with 2 user roles : **admin** and **manager**.
The first one is considered as the super-user : it can do everything. 
The second one has lower permissions so you can give access to someone that should not access everything can work with the solution.

You can of course add your own roles to the application. To do so, open the **config/authorize.yaml** file and a new role in the `roles` property. (**Important : roles should be lower-case and only alphabetic caracters**).

Each roles has two possibles rule keys : `controllers` and `permissions`.
 
- `controllers` gives you the ability to limit access to controller's action to a given role. [Full controllers and actions names available here](#TODO)
- `permissions` gives you the ability to give permissions to a role. Full [permissions list is available here](#TODO)

#### Creating a new role

##### Controller access

For instance, if you want to create a role `accountant` that can access customers and invoices, you would add the following parameters in the **config/authorize.yaml** file, under the `roles` key :

```yaml
accountant:
    controllers:
        customers:
            index: true
            add: true
            edit: true
            view: true
            delete: true
        invoices:
            index: true
            add: true
            delete: true
```

If the actions list is composed of all the actions available for a controller, you can use the special wildcard character **\*** next to the controller name :

```yaml
accountant:
    controllers:
        customers: '*'
        invoices:
            index: true
            add: true
            delete: true
```

Be sure to wrap it around single quotes, otherwise, the yaml parser will not recognize the star character as valid string.

If you need to define common controller permissions, you can use the special role `all` which will be copied in every role configured in your yaml file. Be aware that you can override those values if needed by defining only the counter-examples specific to the roles.

For instance, consider the following file :

```yaml
authorize:
    roles:
        all:
            controllers:
                dashboard: '*'
                plans:
                    index: true
                    add: true
                    edit: true
                    delete: true
                users:
                    logout: true
        
        accountant:
            controllers:
                plans:
                    delete: false
```

All roles will be granted permission to access the `index`, `add`, `edit` and `delete` action of the plans controller. The `accountant` role, however, will be denied access to the `delete` action but granted to all others. 

##### Permissions

Defining permissions is as easy as granting controller access. Under the `permissions` key (located at the same level as the `controllers` key), you can define your permissions.

### TODO : AJOUTER DES EXEMPLES QUAND ON AURA UNE LISTE DE PERMISSIONS A UTILISER