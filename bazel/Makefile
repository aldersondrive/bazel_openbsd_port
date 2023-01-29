VERSION =		6.0.0

COMMENT =		polyglot build tool
HOMEPAGE =		https://bazel.build/

DISTNAME =		bazel-${VERSION}

CATEGORIES =		devel

# Apache 2.0.
PERMIT_PACKAGE =	Yes

WANTLIB +=		${COMPILER_LIBCXX} c m
DPB_PROPERTIES =	parallel

MASTER_SITES =		https://github.com/bazelbuild/bazel/releases/download/${VERSION}/
EXTRACT_SUFX =		-dist.zip
FIX_CLEANUP_PERMISSIONS	=	Yes

COMPILER =		base-clang ports-gcc

MODULES =		java lang/python
MODJAVA_VER =		11

BUILD_DEPENDS =		archivers/zip \
			shells/bash

MAKE_ENV =		BAZEL_JAVAC_OPTS="-J-Xmx2g -J-Xms200m" \
			EMBED_LABEL="${VERSION}" \
			EXTRA_BAZEL_ARGS="--jobs=${MAKE_JOBS} --tool_java_runtime_version=local_jdk" \
			PATH="${LOCALBASE}/bin:${PATH}" \
			SOURCE_DATE_EPOCH="0" \
			TMPDIR="${WRKDIR}/tmp" \
			VERBOSE="yes"

WRKDIST =		${WRKDIR}

# Disable stripping. The `bazel` binary contains an embedded zip file;
# stripping the binary removes this zip file, breaking the binary.
INSTALL_STRIP =

post-patch:
	cp ${FILESDIR}/extra-patch-* ${WRKDIR}/third_party/

do-build:
	mkdir -p ${WRKDIR}/tmp
	cd ${WRKBUILD} && env -i ${MAKE_ENV} ${LOCALBASE}/bin/bash ./compile.sh

do-install:
	${INSTALL_PROGRAM} ${WRKDIR}/output/bazel ${PREFIX}/bin
	${INSTALL_DATA_DIR} ${PREFIX}/share/examples/bazel/
	${INSTALL_DATA} files/bazel.bazelrc \
		${PREFIX}/share/examples/bazel/bazel.bazelrc


.include <bsd.port.mk>
