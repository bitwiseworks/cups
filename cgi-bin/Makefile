#
# CGI makefile for CUPS.
#
# Copyright 2007-2018 by Apple Inc.
# Copyright 1997-2006 by Easy Software Products.
#
# These coded instructions, statements, and computer programs are the
# property of Apple Inc. and are protected by Federal copyright
# law.  Distribution and use rights are outlined in the file "LICENSE.txt"
# which should have been included with this file.  If this file is
# missing or damaged, see the license at "http://www.cups.org/".
#

include ../Makedefs

LIBOBJS	=	\
		help-index.o \
		html.o \
		ipp-var.o \
		search.o \
		template.o \
		var.o
OBJS	=	\
		$(LIBOBJS) \
		admin.o \
		classes.o \
		help.o \
		jobs.o \
		printers.o \
		testcgi.o \
		testhi.o \
		testtemplate.o
CGIS	=	\
		admin.cgi \
		classes.cgi \
		help.cgi \
		jobs.cgi \
		printers.cgi
LIBTARGETS =	\
		libcupscgi.a

UNITTARGETS =	\
		testcgi$(EXEEXT) \
		testhi$(EXEEXT) \
		testtemplate$(EXEEXT)

TARGETS	=	\
		$(LIBTARGETS) \
		$(CGIS)


#
# Make all targets...
#

all:	$(TARGETS)


#
# Make library targets...
#

libs:		$(LIBTARGETS) $(UNITTESTS)


#
# Make unit tests...
#

unittests:	$(UNITTARGETS)


#
# Clean all object files...
#

clean:
	$(RM) $(OBJS) $(TARGETS) $(UNITTARGETS)


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
	$(INSTALL_DIR) -m 755 $(SERVERBIN)/cgi-bin
	for file in $(CGIS); do \
		$(INSTALL_BIN) $$file $(SERVERBIN)/cgi-bin; \
	done
	if test "x$(SYMROOT)" != "x"; then \
		$(INSTALL_DIR) $(SYMROOT); \
		for file in $(CGIS); do \
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
# Uninstall all targets...
#

uninstall:
	for file in $(CGIS); do \
		$(RM) $(SERVERBIN)/cgi-bin/$$file; \
	done
	-$(RMDIR) $(SERVERBIN)/cgi-bin


#
# libcupscgi.a
#

libcupscgi.a:	$(LIBOBJS)
	echo Archiving $@...
	$(RM) $@
	$(AR) $(ARFLAGS) $@ $(LIBOBJS)
	$(RANLIB) $@


#
# admin.cgi
#

admin.cgi:	admin.o ../Makedefs ../cups/$(LIBCUPS) libcupscgi.a
	echo Linking $@...
	$(LD_CC) $(LDFLAGS) -Zexe -o $@ admin.o libcupscgi.a $(LIBS)
	$(CODE_SIGN) -s "$(CODE_SIGN_IDENTITY)" $@
	$(RM) admin


#
# classes.cgi
#

classes.cgi:	classes.o ../Makedefs ../cups/$(LIBCUPS) libcupscgi.a
	echo Linking $@...
	$(LD_CC) $(LDFLAGS) -Zexe -o $@ classes.o libcupscgi.a $(LIBS)
	$(CODE_SIGN) -s "$(CODE_SIGN_IDENTITY)" $@
	$(RM) classes


#
# help.cgi
#

help.cgi:	help.o ../Makedefs ../cups/$(LIBCUPS) libcupscgi.a
	echo Linking $@...
	$(LD_CC) $(LDFLAGS) -Zexe -o $@ help.o libcupscgi.a $(LIBS)
	$(CODE_SIGN) -s "$(CODE_SIGN_IDENTITY)" $@
	$(RM) help


#
# jobs.cgi
#

jobs.cgi:	jobs.o ../Makedefs ../cups/$(LIBCUPS) libcupscgi.a
	echo Linking $@...
	$(LD_CC) $(LDFLAGS) -Zexe -o $@ jobs.o libcupscgi.a $(LIBS)
	$(CODE_SIGN) -s "$(CODE_SIGN_IDENTITY)" $@
	$(RM) jobs


#
# printers.cgi
#

printers.cgi:	printers.o ../Makedefs ../cups/$(LIBCUPS) libcupscgi.a
	echo Linking $@...
	$(LD_CC) $(LDFLAGS) -Zexe -o $@ printers.o -L. libcupscgi.a $(LIBS)
	$(CODE_SIGN) -s "$(CODE_SIGN_IDENTITY)" $@
	$(RM) printers


#
# testcgi
#

testcgi$(EXEEXT):	testcgi.o ../Makedefs libcupscgi.a ../cups/$(LIBCUPSSTATIC)
	echo Linking $@...
	$(LD_CC) $(ARCHFLAGS) $(LDFLAGS) -o $@ testcgi.o libcupscgi.a \
		../cups/$(LIBCUPSSTATIC) $(COMMONLIBS) $(SSLLIBS) $(DNSSDLIBS) \
		$(LIBZ) $(LIBGSSAPI)
	$(CODE_SIGN) -s "$(CODE_SIGN_IDENTITY)" $@
	echo Testing CGI API...
	./testcgi$(EXEEXT)


#
# testhi
#

testhi$(EXEEXT):	testhi.o ../Makedefs libcupscgi.a ../cups/$(LIBCUPSSTATIC)
	echo Linking $@...
	$(LD_CC) $(ARCHFLAGS) $(LDFLAGS) -o $@ testhi.o libcupscgi.a \
		../cups/$(LIBCUPSSTATIC) $(COMMONLIBS) $(SSLLIBS) $(DNSSDLIBS) \
		$(LIBZ) $(LIBGSSAPI)
	$(CODE_SIGN) -s "$(CODE_SIGN_IDENTITY)" $@
	echo Testing help index API...
	./testhi$(EXEEXT)


#
# testtemplate
#

testtemplate$(EXEEXT):	testtemplate.o ../Makedefs libcupscgi.a ../cups/$(LIBCUPSSTATIC)
	echo Linking $@...
	$(LD_CC) $(LDFLAGS) -o $@ testtemplate.o libcupscgi.a ../cups/$(LIBCUPSSTATIC) \
		$(COMMONLIBS) $(SSLLIBS) $(DNSSDLIBS) $(LIBZ) $(LIBGSSAPI)
	$(CODE_SIGN) -s "$(CODE_SIGN_IDENTITY)" $@


#
# Dependencies...
#

include Dependencies
