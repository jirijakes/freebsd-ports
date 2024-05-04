PORTNAME=          mempool-frontend
DISTVERSION=       2.5.0
DISTVERSIONPREFIX= v
CATEGORIES=        net-p2p

MAINTAINER=        jiri@jirijakes.com
WWW=               https://mempool.space
LICENSE=           AGPLv3

USE_GITHUB=        yes
GH_ACCOUNT=        mempool
GH_PROJECT=        mempool
WRKSRC_SUBDIR=     frontend

NO_ARCH=           yes

OPTIONS_DEFINE=    EXAMPLES

USES=              nodejs

USERS=             ${WWWOWN}
GROUPS=            ${WWWGRP}

PLIST_SUB=         WWWOWN="${WWWOWN}" WWWGRP="${WWWGRP}"

do-build:
	cd ${WRKSRC} && npm install && npm run build

do-install:
	cd ${WRKSRC}/dist/mempool/ && ${COPYTREE_SHARE} . ${STAGEDIR}${WWWDIR}

do-install-EXAMPLES-on:
	cd ${WRKSRC}/.. \
	&& ${CP} nginx-mempool.conf nginx-mempool.conf.sample \
	&& ${SED} -e "s#root.*#root ${WWWDIR}/browser;#g" -i "" nginx-mempool.conf.sample \
	&& ${SED} -n "/map.*cookie_lang/,/}/p" nginx.conf > nginx-mempool.map.sample \
	&& ${SED} -n "/map.*http_accept_language/,/}/p" nginx.conf >> nginx-mempool.map.sample
	${MKDIR} ${STAGEDIR}${EXAMPLESDIR}
	${INSTALL_DATA} ${WRKSRC}/../nginx-mempool.conf.sample ${STAGEDIR}${EXAMPLESDIR}
	${INSTALL_DATA} ${WRKSRC}/../nginx-mempool.map.sample ${STAGEDIR}${EXAMPLESDIR}

.include <bsd.port.mk>
