#
# Filter makefile for CUPS.
#
# Copyright 2007-2017 by Apple Inc.
# Copyright 1997-2006 by Easy Software Products.
#
# These coded instructions, statements, and computer programs are the
# property of Apple Inc. and are protected by Federal copyright
# law.  Distribution and use rights are outlined in the file "LICENSE.txt"
# which should have been included with this file.  If this file is
# missing or damaged, see the license at "http://www.cups.org/".
#
# This file is subject to the Apple OS-Developed Software exception.
#

include ../Makedefs

DEFFILE =	cupsimage.def
FILTERS	=	\
		commandtops$(EXEEXT) \
		gziptoany$(EXEEXT) \
		pstops$(EXEEXT) \
		rastertoepson$(EXEEXT) \
		rastertohp$(EXEEXT) \
		rastertolabel$(EXEEXT) \
		rastertopwg$(EXEEXT)
LIBTARGETS =	\
		$(LIBCUPSIMAGE) \
		libcupsimage.a
UNITTARGETS =	\
		rasterbench$(EXEEXT) \
		testclient$(EXEEXT) \
		testraster$(EXEEXT)
TARGETS	=	\
		$(LIBTARGETS) \
		$(FILTERS)

IMAGEOBJS =	error.o interpret.o raster.o
OBJS	=	$(IMAGEOBJS) \
		commandtops.o gziptoany.o common.o pstops.o \
		rasterbench.o rastertoepson.o rastertohp.o rastertolabel.o \
		rastertopwg.o testclient.o testraster.o


#
# Make all targets...
#

all:	$(TARGETS)


#
# Make library targets...
#

libs:		$(LIBTARGETS)


#
# Make unit tests...
#

unittests:	$(UNITTARGETS)


#
# Clean all object files...
#

clean:
	$(RM) $(OBJS) $(TARGETS) $(UNITTARGETS)
	$(RM) libcupsimage.so libcupsimage.sl libcupsimage.dylib
	$(RM) $(DEFFILE)


#
# Update dependencies (without system header dependencies...)
#

depend:
	$(CC) -MM $(ALL_CFLAGS) $(OBJS:.o=.c) >Dependencies


#
# Install all targets...
#

install:	all install-data install-headers install-libs install-exec


#
# Install data files...
#

install-data:


#
# Install programs...
#

install-exec:
	$(INSTALL_DIR) -m 755 $(SERVERBIN)/filter
	for file in $(FILTERS); do \
		$(INSTALL_BIN) $$file $(SERVERBIN)/filter; \
	done
	$(RM) $(SERVERBIN)/filter/rastertodymo.exe
	$(LN) rastertolabel$(EXEEXT) $(SERVERBIN)/filter/rastertodymo
	if test "x$(SYMROOT)" != "x"; then \
		$(INSTALL_DIR) $(SYMROOT); \
		for file in $(FILTERS); do \
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

install-libs: $(INSTALLSTATIC)
	$(INSTALL_DIR) -m 755 $(LIBDIR)
	$(INSTALL_LIB) $(LIBCUPSIMAGE) $(LIBDIR)
	$(INSTALL) libcupsimage.a $(LIBDIR)
	-if test $(LIBCUPSIMAGE) = "libcupsimage.so.2" -o $(LIBCUPSIMAGE) = "libcupsimage.sl.2"; then \
		$(RM) $(LIBDIR)/`basename $(LIBCUPSIMAGE) .2`; \
		$(LN) $(LIBCUPSIMAGE) $(LIBDIR)/`basename $(LIBCUPSIMAGE) .2`; \
	fi
	-if test $(LIBCUPSIMAGE) = "libcupsimage.2.dylib"; then \
		$(RM) $(LIBDIR)/libcupsimage.dylib; \
		$(LN) $(LIBCUPSIMAGE) $(LIBDIR)/libcupsimage.dylib; \
	fi
	if test "x$(SYMROOT)" != "x"; then \
		$(INSTALL_DIR) $(SYMROOT); \
		cp $(LIBCUPSIMAGE) $(SYMROOT); \
		dsymutil $(SYMROOT)/$(LIBCUPSIMAGE); \
	fi

installstatic:
	$(INSTALL_DIR) -m 755 $(LIBDIR)
	$(INSTALL_LIB) -m 755 libcupsimage.a $(LIBDIR)
	$(RANLIB) $(LIBDIR)/libcupsimage.a
	$(CHMOD) 555 $(LIBDIR)/libcupsimage.a


#
# Uninstall all targets...
#

uninstall:
	for file in $(FILTERS); do \
		$(RM) $(SERVERBIN)/filter/$$file; \
	done
	$(RM) $(SERVERBIN)/filter/rastertodymo
	-$(RMDIR) $(SERVERBIN)/filter
	-$(RMDIR) $(SERVERBIN)
	$(RM) $(LIBDIR)/libcupsimage.2.dylib
	$(RM) $(LIBDIR)/libcupsimage.a
	$(RM) $(LIBDIR)/libcupsimage.dylib
	$(RM) $(LIBDIR)/libcupsimage_s.a
	$(RM) $(LIBDIR)/libcupsimage.sl
	$(RM) $(LIBDIR)/libcupsimage.sl.2
	$(RM) $(LIBDIR)/libcupsimage.so
	$(RM) $(LIBDIR)/libcupsimage.so.2
	-$(RMDIR) $(LIBDIR)


#
# Automatic API help files...
#

apihelp:
	echo Generating CUPS API help files...
	mxmldoc --section "Programming" --title "Raster API" \
		--css ../doc/cups-printable.css \
		--header api-raster.header --intro api-raster.shtml \
		api-raster.xml \
		../cups/raster.h interpret.c raster.c \
		>../doc/help/api-raster.html
	mxmldoc --tokens help/api-raster.html api-raster.xml >../doc/help/api-raster.tokens
	$(RM) api-raster.xml
	mxmldoc --section "Programming" \
		--title "Developing PostScript Printer Drivers" \
		--css ../doc/cups-printable.css \
		--header postscript-driver.header \
		--intro postscript-driver.shtml \
		>../doc/help/postscript-driver.html
	mxmldoc --section "Programming" \
		--title "Introduction to the PPD Compiler" \
		--css ../doc/cups-printable.css \
		--header ppd-compiler.header \
		--intro ppd-compiler.shtml \
		>../doc/help/ppd-compiler.html
	mxmldoc --section "Programming" \
		--title "Developing Raster Printer Drivers" \
		--css ../doc/cups-printable.css \
		--header raster-driver.header \
		--intro raster-driver.shtml \
		>../doc/help/raster-driver.html
	mxmldoc --section "Specifications" \
		--title "CUPS PPD Extensions" \
		--css ../doc/cups-printable.css \
		--header spec-ppd.header \
		--intro spec-ppd.shtml \
		>../doc/help/spec-ppd.html


#
# commandtops
#

commandtops$(EXEEXT):	commandtops.o ../cups/$(LIBCUPS)
	echo Linking $@...
	$(LD_CC) $(LDFLAGS) -o $@ commandtops.o $(LIBS)
	$(CODE_SIGN) -s "$(CODE_SIGN_IDENTITY)" $@


#
# gziptoany
#

gziptoany$(EXEEXT):	gziptoany.o ../Makedefs ../cups/$(LIBCUPS)
	echo Linking $@...
	$(LD_CC) $(LDFLAGS) -o $@ gziptoany.o $(LIBZ) $(LIBS)
	$(CODE_SIGN) -s "$(CODE_SIGN_IDENTITY)" $@


#
# libcupsimage.so.2, libcupsimage.sl.2
#

$(LIBCUPSIMAGE) libcupsimage.sl.2:	$(IMAGEOBJS) $(DEFFILE)
	echo Linking $@...
	$(DSO) $(ARCHFLAGS) $(LDFLAGS) $(DSOFLAGS) -o $@ $(DEFFILE) $(IMAGEOBJS) $(DSOLIBS) \
		-L../cups $(LINKCUPS)
	$(CODE_SIGN) -s "$(CODE_SIGN_IDENTITY)" $@
#	$(RM) `basename $@ .2`
#	$(LN) $@ `basename $@ .2`

$(DEFFILE):
	echo Creating $@...
	echo "LIBRARY cupsim INITINSTANCE TERMINSTANCE" > $@
	echo "DESCRIPTION \"$(BUILDLEVEL_INFO)@@cupsimage\"" >> $@
	echo "DATA MULTIPLE" >> $@
	echo "EXPORTS" >> $@
	emxexp $(IMAGEOBJS) >> $@

#
# libcupsimage.2.dylib
#

libcupsimage.2.dylib:	$(IMAGEOBJS) $(LIBCUPSIMAGEORDER)
	echo Linking $@...
	$(DSO) $(ARCHFLAGS) $(LDFLAGS) $(DSOFLAGS) -o $@ \
		-install_name $(libdir)/$@ \
		-current_version 2.3.0 \
		-compatibility_version 2.0.0 \
		$(IMAGEOBJS) $(DSOLIBS) -L../cups $(LINKCUPS)
	$(CODE_SIGN) -s "$(CODE_SIGN_IDENTITY)" $@
	$(RM) libcupsimage.dylib
	$(LN) $@ libcupsimage.dylib


#
# libcupsimage.la
#

libcupsimage.la:       $(IMAGEOBJS)
	echo Linking $@...
	$(DSO) $(ARCHFLAGS) $(LDFLAGS) $(DSOFLAGS) -o $@ $(IMAGEOBJS:.o=.lo) $(DSOLIBS) \
		-L../cups $(LINKCUPS) \
		-rpath $(LIBDIR) -version-info 2:3


#
# libcupsimage.a
#

libcupsimage.a:	$(IMAGEOBJS)
	echo Archiving $@...
	$(RM) $@
#	$(AR) $(ARFLAGS) $@ $(IMAGEOBJS)
#	$(RANLIB) $@
	emximp -o $@ $(LIBCUPSIMAGE)


#
# pstops
#

pstops$(EXEEXT):	pstops.o common.o ../cups/$(LIBCUPS)
	echo Linking $@...
	$(LD_CC) $(LDFLAGS) -o $@ pstops.o common.o $(LIBS)
	$(CODE_SIGN) -s "$(CODE_SIGN_IDENTITY)" $@


#
# rastertoepson
#

rastertoepson$(EXEEXT):	rastertoepson.o ../cups/$(LIBCUPS) $(LIBCUPSIMAGE)
	echo Linking $@...
	$(LD_CC) $(LDFLAGS) -o $@ rastertoepson.o $(LINKCUPSIMAGE) $(IMGLIBS) $(LIBS)
	$(CODE_SIGN) -s "$(CODE_SIGN_IDENTITY)" $@


#
# rastertohp
#

rastertohp$(EXEEXT):	rastertohp.o ../cups/$(LIBCUPS) $(LIBCUPSIMAGE)
	echo Linking $@...
	$(LD_CC) $(LDFLAGS) -o $@ rastertohp.o $(LINKCUPSIMAGE) $(IMGLIBS) $(LIBS)
	$(CODE_SIGN) -s "$(CODE_SIGN_IDENTITY)" $@


#
# rastertolabel
#

rastertolabel$(EXEEXT):	rastertolabel.o ../cups/$(LIBCUPS) $(LIBCUPSIMAGE)
	echo Linking $@...
	$(LD_CC) $(LDFLAGS) -o $@ rastertolabel.o $(LINKCUPSIMAGE) $(IMGLIBS) $(LIBS)
	$(CODE_SIGN) -s "$(CODE_SIGN_IDENTITY)" $@


#
# rastertopwg
#

rastertopwg$(EXEEXT):	rastertopwg.o ../cups/$(LIBCUPS) $(LIBCUPSIMAGE)
	echo Linking $@...
	$(LD_CC) $(LDFLAGS) -o $@ rastertopwg.o $(LINKCUPSIMAGE) $(IMGLIBS) $(LIBS)
	$(CODE_SIGN) -s "$(CODE_SIGN_IDENTITY)" $@

rastertopwg-static$(EXEEXT):	rastertopwg.o ../cups/$(LIBCUPSSTATIC) libcupsimage.a
	echo Linking $@...
	$(LD_CC) $(LDFLAGS) -o $@ rastertopwg.o libcupsimage.a \
		../cups/$(LIBCUPSSTATIC) $(IMGLIBS) $(DSOLIBS) $(COMMONLIBS) \
		$(SSLLIBS) $(DNSSDLIBS) $(LIBGSSAPI)
	$(CODE_SIGN) -s "$(CODE_SIGN_IDENTITY)" $@


#
# testclient (dependency on static libraries is intentional)
#

testclient$(EXEEXT):	testclient.o ../cups/$(LIBCUPSSTATIC) libcupsimage.a
	echo Linking $@...
	$(LD_CC) $(LDFLAGS) -o $@ testclient.o \
		libcupsimage.a ../cups/$(LIBCUPSSTATIC) \
		$(LIBGSSAPI) $(SSLLIBS) $(DNSSDLIBS) $(COMMONLIBS) $(LIBZ)
	$(CODE_SIGN) -s "$(CODE_SIGN_IDENTITY)" $@


#
# testraster
#

testraster$(EXEEXT):	testraster.o ../cups/$(LIBCUPSSTATIC) libcupsimage.a
	echo Linking $@...
	$(LD_CC) $(ARCHFLAGS) $(LDFLAGS) -o $@ testraster.o libcupsimage.a \
		../cups/$(LIBCUPSSTATIC) $(IMGLIBS) $(DSOLIBS) $(COMMONLIBS) \
		$(SSLLIBS) $(DNSSDLIBS) $(LIBGSSAPI)
	$(CODE_SIGN) -s "$(CODE_SIGN_IDENTITY)" $@
	echo Running raster API tests...
	./testraster


#
# rasterbench
#

rasterbench$(EXEEXT):	rasterbench.o libcupsimage.a
	echo Linking $@...
	$(LD_CC) $(LDFLAGS) -o $@ rasterbench.o libcupsimage.a $(LIBS)
	$(CODE_SIGN) -s "$(CODE_SIGN_IDENTITY)" $@


#
# Dependencies...
#

include Dependencies
