# $OpenBSD: Makefile,v$

VERSION =		3.2.0

COMMENT =		polyglot build tool
HOMEPAGE =		https://bazel.build/

DISTNAME =		bazel-${VERSION}

CATEGORIES =		devel

# Apache 2.0 license.
PERMIT_PACKAGE =	Yes

WANTLIB +=		${COMPILER_LIBCXX} c m

MASTER_SITES =		https://github.com/bazelbuild/bazel/releases/download/${VERSION}/
EXTRACT_SUFX =		-dist.zip

COMPILER =		base-clang ports-gcc

MODULES =		java lang/python
MODJAVA_VER =		1.8
MODPY_VERSION =		${MODPY_DEFAULT_VERSION_3}

BUILD_DEPENDS =		archivers/zip \
			shells/bash

# Disable stripping. The `bazel` binary contains an embedded zip file;
# stripping the binary removes this zip file, breaking the binary.
INSTALL_STRIP =

post-patch:
	echo 'build --host_javabase=@local_jdk//:jdk' \
		>> ${WRKDIR}/scripts/packages/bazel.bazelrc
	echo 'test --host_javabase=@local_jdk//:jdk' \
		>> ${WRKDIR}/scripts/packages/bazel.bazelrc
	echo 'run --host_javabase=@local_jdk//:jdk' \
		>> ${WRKDIR}/scripts/packages/bazel.bazelrc

do-build:
	cd ${WRKDIR} && ${SETENV} \
		EMBED_LABEL="${VERSION}" \
		EXTRA_BAZEL_ARGS="--host_javabase=@local_jdk//:jdk" \
		PATH="${LOCALBASE}/bin:${PATH}" \
		SOURCE_DATE_EPOCH="0" \
		${LOCALBASE}/bin/bash ./compile.sh

do-install:
	${INSTALL_PROGRAM} ${WRKDIR}/output/bazel ${PREFIX}/bin
	${INSTALL_DATA_DIR} ${PREFIX}/share/examples/bazel/
	${INSTALL_DATA} ${WRKDIR}/scripts/packages/bazel.bazelrc \
		${PREFIX}/share/examples/bazel/bazel.bazelrc

.include <bsd.port.mk>
