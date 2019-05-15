# odoo8-postgresl-qa

L'image est basée sur Centos 7 et expose les variables d'environnement suivant:

* _ODOO_RPM_URL_: une url valide vers le RPM d'Odoo a installer
* _ODOO_SRC_URL_: une url valide vers le source (tar.gz) d'Odoo à installer
* _PGDATA_: chemin vers le répertoire `data` de postgresql. Pointe vers `/usr/lib/pgsql/data` par défaut

L'image se repose sur l'image intermédiaire [centos7-without-systemd](https://hub.docker.com/r/tahitiwebdesign/centos7-without-systemd).
Cette dernière désactive systemd qui peut poser des problèmes de sécurité lorsqu'utilisé depuis un container.

Exemple d'utilisation de l'image:

```bash
docker run --name odoo8-qa -d -p 8069:8069 -p 5432:5432 \
    -e ODOO_RPM_URL="http://cdn.tahiti-web-management.com/odoo_8.0.20171001.noarch.rpm" \
    -e ODOO_SRC_URL="http://cdn.tahiti-web-management.com/odoo_8.0.20171001.tar.gz" \
    paraita/odoo8-postgresql-qa
```

## Postgresql

Postgresql 9.3 est installé depuis les packages officiels.
La base `odoo` est crée. Les utilisateurs suivants sont crées et autorisés à accéder à cette base (_user/password_):

* admin/admin
* odoo/odoo
* postgres

Le port par défaut de Postgresql (5432) est exposé au container.

Postgresql se lance avec:

```bash
su postgres -c "pg_ctl -D $PGDATA start"
```

## Odoo

Odoo 8.0 est installé avec le RPM officiel.
Le code source d'Odoo 8.0 est aussi utilisé afin de pouvoir y exécuter des tests.

Le répertoire d'addons d'Odoo est accessible sur `/odoo/addons`. Ce répertoire est montable depuis la machine hôte.
Le répertoire config est accessible sur `/odoo/config` et contient le fichier de configuration par défaut d'odoo. Il peut être monté depuis la machine hôte.

Pour démarrer Odoo:

```bash
su odoo -c "openerp-server -c /odoo/config/openerp-server.conf \
                           --log-level debug"
```



