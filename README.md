# Custom Odoo Docker Setup

Set of docker images and some sample docker-compose files for Odoo.

Directories layout:

    ├── base :       Base image for Odoo, OCB and OpenUpgrade
    ├── ocb-oca :    docker image for **OCB + OCA modules**
    ├── openupgrade-oca : docker image for **OpenUpgrade + OCA modules**
    ├── odoo-ee :    docker image for **Odoo EE**


## Update the images in the CI

By default during the rebuild, the previous images are used. If you want to
trigger a full rebuild of the base Debian image, the Odoo code and modules
code, you just have to update the LASTBUILD variable in the Dockerfile of the
debian base image

## Locally build the docker images

First checkout this repository:

    git clone https://github.com/anybox/odoo-docker
    cd odoo-docker

Choose the version you want:

    git checkout 12.0

Read the help of the available commands

    make help

Build the base Odoo image:

    cd odoo
    make build

Either build the image with additional OCA modules:

    cd odoo-oca
    make build

Or build the image with only Enterprise Edition:

    cd odoo-ee
    make build


### Start Odoo and PostgreSQL

    make init
    make run

Then open on a browser on your local machine:

    firefox http://localhost:8069

### Stop Odoo

    make stop

### Destroy the application and all data

    make destroy

### Restore db

    docker cp backup.dump odoo_db_1:/tmp/
    docker-compose exec --user db postgresql pg_restore --role odoo -O -Fc -d odoo /tmp/<dumpfile>
    docker-compose exec postgresql rm /tmp/<dumpfile>

### Update all

    docker-compose run --rm odoo -u all --stop-after-init

### Reset password

    docker-compose exec --user db postgresql psql odoo -c "update res_users set password='admin' where login='admin';"

