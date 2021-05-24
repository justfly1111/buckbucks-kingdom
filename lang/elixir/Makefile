# Created by: Li-Wen Hsu <lwhsu@FreeBSD.org>

PORTNAME=	elixir
DISTVERSIONPREFIX=	v
DISTVERSION=	1.12.0
CATEGORIES=	lang
MASTER_SITES+=	https://github.com/elixir-lang/elixir/releases/download/${DISTVERSIONPREFIX}${PORTVERSION}/:doc
DISTFILES=	Docs.zip:doc
DIST_SUBDIR=	elixir/${PORTVERSION}

MAINTAINER=	erlang@FreeBSD.org
COMMENT=	Functional, meta-programming aware language built on top of Erlang VM

LICENSE=	APACHE20

BUILD_DEPENDS=	erlang-runtime${ERLANG_VER}>0:lang/erlang-runtime${ERLANG_VER}
RUN_DEPENDS=	erlang-runtime${ERLANG_VER}>0:lang/erlang-runtime${ERLANG_VER}

USES=		gmake

USE_GITHUB=	yes
GH_ACCOUNT=	elixir-lang

USE_LOCALE=	en_US.UTF-8
ALL_TARGET=	# empty
REINPLACE_ARGS=	-i ""
NO_ARCH=	yes
OPTIONS_SUB=	yes
MAKE_ENV+=	MAN_PREFIX=${PREFIX}/man \
		PATH="${LOCALBASE}/lib/erlang${ERLANG_VER}/bin:${PATH}" \

OPTIONS_DEFINE=	SRC DOCS
OPTIONS_DEFAULT=SRC

SRC_DESC=	Install source code

ERLANG_VER=	23

.include <bsd.port.options.mk>

post-extract:
	@${FIND} ${WRKDIR}/doc -name .build -delete

post-patch:
	@${REINPLACE_CMD} -e \
		's|^ERTS_BIN=.*$$|ERTS_BIN=${PREFIX}/lib/erlang${ERLANG_VER}/bin/|' \
		${WRKSRC}/bin/elixir

post-install-DOCS-on:
	${MKDIR} ${STAGEDIR}${DOCSDIR}
	cd ${WRKDIR}/doc/ && ${COPYTREE_SHARE} \* ${STAGEDIR}${DOCSDIR}

post-install-SRC-on:
	${MKDIR} ${STAGEDIR}${PREFIX}/lib/elixir/lib
	cd ${WRKSRC}/lib/ && for lib in *; do \
		${MKDIR} ${STAGEDIR}${PREFIX}/lib/elixir/lib/$${lib}/lib; \
		cd ${WRKSRC}/lib/$${lib}/lib && \
			${COPYTREE_SHARE} \* ${STAGEDIR}${PREFIX}/lib/elixir/lib/$${lib}/lib; \
	done

do-test:
	cd ${WRKSRC} && ${SETENV} ${MAKE_ENV} ${MAKE_CMD} test

.include <bsd.port.mk>
