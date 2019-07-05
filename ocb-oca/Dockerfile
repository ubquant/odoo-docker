FROM registry.anybox.cloud/anybox/odoo-docker/ocb:12.0

LABEL maintainer="Anybox"
ENV VERSION 12.0

RUN set -x; \
    cd /srv/extra-addons \
#
    ### DOWNLOAD MODULES
    && for repo in \
        OCA/web \
        OCA/server-brand \
        OCA/server-tools \
        Openworx/backend_theme \
        OCA/website \
        OCA/contract \
        OCA/project \
        OCA/l10n-france \
        OCA/partner-contact \
    ; do curl -o addons.zip -sSL https://github.com/$repo/archive/$VERSION.zip \
        && unzip addons.zip \
        && rm -rf ${repo#*/}-$VERSION/setup \
        && mv -f ${repo#*/}-$VERSION/* . \
        && rm -rf ${repo#*/}-$VERSION \
    ; done \
    && rm -rf .gitignore README.md .travis.yml *zip
            
            
VOLUME /var/lib/odoo
COPY odoo.conf /etc/odoo/