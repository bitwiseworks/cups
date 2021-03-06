#
# Makefile for the CUPS PPD Compiler.
#
# Copyright 2007-2018 by Apple Inc.
# Copyright 2002-2006 by Easy Software Products.
#
# These coded instructions, statements, and computer programs are the
# property of Apple Inc. and are protected by Federal copyright
# law.  Distribution and use rights are outlined in the file "LICENSE.txt"
# which should have been included with this file.  If this file is
# missing or damaged, see the license at "http://www.cups.org/".
#

#
# Include standard definitions...
#

include ../Makedefs


#
# Object files...
#
LIBOBJS =	\
		ppdc-array.o \
		ppdc-attr.o \
		ppdc-catalog.o \
		ppdc-choice.o \
		ppdc-constraint.o \
		ppdc-driver.o \
		ppdc-file.o \
		ppdc-filter.o \
		ppdc-font.o \
		ppdc-group.o \
		ppdc-import.o \
		ppdc-mediasize.o \
		ppdc-message.o \
		ppdc-option.o \
		ppdc-profile.o \
		ppdc-shared.o \
		ppdc-source.o \
		ppdc-string.o \
		ppdc-variable.o
OBJS =		\
		$(LIBOBJS) \
		genstrings.o \
		ppdc.o \
		ppdhtml.o \
		ppdi.o \
		ppdmerge.o \
		ppdpo.o \
		testcatalog.o
LIBTARGETS =	\
		libcupsppdc.a
UNITTARGETS =	\
		ppdc-static$(EXEEXT) \
		ppdi-static$(EXEEXT) \
		testcatalog$(EXEEXT)
EXECTARGETS =	\
		ppdc$(EXEEXT) \
		ppdhtml$(EXEEXT) \
		ppdi$(EXEEXT) \
		ppdmerge$(EXEEXT) \
		ppdpo$(EXEEXT)

TARGETS	=	\
		$(LIBTARGETS) \
		$(EXECTARGETS) \
		$(LOCALTARGET)


#
# Make everything...
#

all:		$(TARGETS)


#
# Make library targets...
#

libs:		$(LIBTARGETS)


#
# Make unit tests...
#

unittests:	$(UNITTARGETS)


#
# Clean everything...
#

clean:
	$(RM) $(OBJS) core
	$(RM) *.bak *.bck core.*
	$(RM) $(TARGETS) $(UNITTARGETS) genstrings$(EXEEXT)
	$(RM) -r ppd ppd2
	$(RM) sample-import.drv sample.c test.drv


#
# Update dependencies...
#

depend:
	$(CXX) -MM $(ALL_CXXFLAGS) $(OBJS:.o=.cxx) >Dependencies


#
# Install all targets...
#

install:	all install-data install-headers install-libs install-exec


#
# Install data files...
#

install-data:
	$(INSTALL_DIR) $(DATADIR)/drv
	$(INSTALL_DATA) sample.drv $(DATADIR)/drv


#
# Install programs...
#

install-exec:
	echo Installing PPD compiler programs...
	$(INSTALL_DIR) $(BINDIR)
	for file in $(EXECTARGETS); do \
		$(INSTALL_BIN) $$file $(BINDIR); \
	done
	if test "x$(SYMROOT)" != "x"; then \
		$(INSTALL_DIR) $(SYMROOT); \
		for file in $(EXECTARGETS); do \
			cp $$file $(SYMROOT); \
			dsymutil $(SYMROOT)/$$file; \
		done \
	fi


#
# Install headers...
#

install-headers:


#
# Install libraries...
#

install-libs:


#
# Uninstall...
#

uninstall:
	for file in $(EXECTARGETS); do \
		$(RM) $(BINDIR)/$$file; \
	done
	$(RM) $(DATADIR)/drv/sample.drv
	$(RMDIR) $(DATADIR)/drv


#
# Local programs (not built when cross-compiling...)
#

local:	genstrings$(EXEEXT)


#
# genstrings - generate GNU gettext strings.
#

genstrings$(EXEEXT):	genstrings.o libcupsppdc.a ../cups/$(LIBCUPSSTATIC) \
			sample.drv ../data/media.defs
	echo Linking $@...
	$(LD_CXX) $(ARCHFLAGS) $(LDFLAGS) -o $@ genstrings.o \
		libcupsppdc.a ../cups/$(LIBCUPSSTATIC) $(LIBGSSAPI) $(SSLLIBS) \
		$(DNSSDLIBS) $(COMMONLIBS) $(LIBZ)
	echo Generating localization strings...
	./genstrings >sample.c


#
# ppdc, the PPD compiler.
#

ppdc$(EXEEXT):		ppdc.o libcupsppdc.a ../cups/$(LIBCUPS)
	echo Linking $@...
	$(LD_CXX) $(LDFLAGS) -o $@ ppdc.o libcupsppdc.a $(LIBS)
	$(CODE_SIGN) -s "$(CODE_SIGN_IDENTITY)" $@


ppdc-static$(EXEEXT):	ppdc.o libcupsppdc.a ../cups/$(LIBCUPSSTATIC) foo.drv foo-fr.po
	echo Linking $@...
	$(LD_CXX) $(ARCHFLAGS) $(LDFLAGS) -o $@ ppdc.o libcupsppdc.a \
		../cups/$(LIBCUPSSTATIC) $(LIBGSSAPI) $(SSLLIBS) $(DNSSDLIBS) \
		$(COMMONLIBS) $(LIBZ)
	$(CODE_SIGN) -s "$(CODE_SIGN_IDENTITY)" $@
	echo Testing PPD compiler...
	./ppdc-static -l en,fr -I ../data foo.drv
	./ppdc-static -l en,fr -z -I ../data foo.drv


#
# ppdhtml, the PPD to HTML utility.
#

ppdhtml$(EXEEXT):	ppdhtml.o libcupsppdc.a ../cups/$(LIBCUPS)
	echo Linking $@...
	$(LD_CXX) $(LDFLAGS) -o $@ ppdhtml.o libcupsppdc.a $(LIBS)
	$(CODE_SIGN) -s "$(CODE_SIGN_IDENTITY)" $@


#
# ppdi, import PPD files.
#

ppdi$(EXEEXT):		ppdi.o libcupsppdc.a ../cups/$(LIBCUPS)
	echo Linking $@...
	$(LD_CXX) $(LDFLAGS) -o $@ ppdi.o libcupsppdc.a $(LIBS)
	$(CODE_SIGN) -s "$(CODE_SIGN_IDENTITY)" $@


ppdi-static$(EXEEXT):	ppdc-static$(EXEEXT) ppdi.o libcupsppdc.a  ../cups/$(LIBCUPSSTATIC)
	echo Linking $@...
	$(LD_CXX) $(ARCHFLAGS) $(LDFLAGS) -o $@ ppdi.o libcupsppdc.a \
		../cups/$(LIBCUPSSTATIC) $(LIBGSSAPI) $(SSLLIBS) $(DNSSDLIBS) \
		$(COMMONLIBS) $(LIBZ)
	$(CODE_SIGN) -s "$(CODE_SIGN_IDENTITY)" $@
	echo Testing PPD importer...
	$(RM) -r ppd ppd2 sample-import.drv
	./ppdc-static -l en -I ../data sample.drv
	./ppdi-static -I ../data -o sample-import.drv ppd/*
	./ppdc-static -l en -I ../data -d ppd2 sample-import.drv
	if diff -r ppd ppd2 >/dev/null; then \
		echo PPD import OK; \
	else \
		echo PPD import FAILED; \
		exit 1; \
	fi


#
# ppdmerge, merge PPD files.
#

ppdmerge$(EXEEXT):	ppdmerge.o ../cups/$(LIBCUPS)
	echo Linking $@...
	$(LD_CXX) $(LDFLAGS) -o $@ ppdmerge.o $(LIBS)
	$(CODE_SIGN) -s "$(CODE_SIGN_IDENTITY)" $@


#
# ppdpo, create message catalog files.
#

ppdpo$(EXEEXT):		ppdpo.o libcupsppdc.a ../cups/$(LIBCUPS)
	echo Linking $@...
	$(LD_CXX) $(LDFLAGS) -o $@ ppdpo.o libcupsppdc.a $(LIBS)
	$(CODE_SIGN) -s "$(CODE_SIGN_IDENTITY)" $@


#
# testcatalog, test ppdcCatalog class.
#

testcatalog$(EXEEXT):	testcatalog.o libcupsppdc.a ../cups/$(LIBCUPSSTATIC)
	echo Linking $@...
	$(LD_CXX) $(LDFLAGS) -o $@ testcatalog.o libcupsppdc.a \
		../cups/$(LIBCUPSSTATIC) $(LIBGSSAPI) $(SSLLIBS) $(DNSSDLIBS) \
		$(COMMONLIBS) $(LIBZ)
	$(CODE_SIGN) -s "$(CODE_SIGN_IDENTITY)" $@


#
# libcupsppdc.a
#

libcupsppdc.a:	$(LIBOBJS)
	echo Archiving $@...
	$(RM) $@
	$(AR) $(ARFLAGS) $@ $(LIBOBJS)
	$(RANLIB) $@

libcupsppdc_s.a:	$(LIBOBJS)
	echo Archiving $@...
	$(RM) $@
	$(AR) $(ARFLAGS) $@ $(LIBOBJS)
	$(RANLIB) $@

#
# Include dependencies...
#

include Dependencies
