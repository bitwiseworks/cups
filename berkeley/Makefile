#
# "$Id: Makefile 10996 2013-05-29 11:51:34Z msweet $"
#
#   Berkeley commands makefile for CUPS.
#
#   Copyright 2007-2012 by Apple Inc.
#   Copyright 1997-2006 by Easy Software Products, all rights reserved.
#
#   These coded instructions, statements, and computer programs are the
#   property of Apple Inc. and are protected by Federal copyright
#   law.  Distribution and use rights are outlined in the file "LICENSE.txt"
#   which should have been included with this file.  If this file is
#   file is missing or damaged, see the license at "http://www.cups.org/".
#

include ../Makedefs


TARGETS	=	lpc$(EXEEXT) lpq$(EXEEXT) lpr$(EXEEXT) lprm$(EXEEXT)
OBJS	=	lpc.o lpq.o lpr.o lprm.o


#
# Make all targets...
#

all:	$(TARGETS)


#
# Make library targets...
#

libs:


#
# Make unit tests...
#

unittests:


#
# Clean all object files...
#

clean:
	$(RM) $(OBJS) $(TARGETS)


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
	echo Installing Berkeley user printing commands in $(BINDIR)...
	$(INSTALL_DIR) -m 755 $(BINDIR)
	$(INSTALL_BIN) lpq$(EXEEXT) $(BINDIR)
	$(INSTALL_BIN) lpr$(EXEEXT) $(BINDIR)
	$(INSTALL_BIN) lprm$(EXEEXT) $(BINDIR)
	echo Installing Berkeley admin printing commands in $(SBINDIR)...
	$(INSTALL_DIR) -m 755 $(SBINDIR)
	$(INSTALL_BIN) lpc$(EXEEXT) $(SBINDIR)
	if test "x$(SYMROOT)" != "x"; then \
		$(INSTALL_DIR) $(SYMROOT); \
		for file in $(TARGETS); do \
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
	$(RM) $(BINDIR)/lpq
	$(RM) $(BINDIR)/lpr
	$(RM) $(BINDIR)/lprm
	$(RM) $(SBINDIR)/lpc
	-$(RMDIR) $(SBINDIR)
	-$(RMDIR) $(BINDIR)


#
# lpc
#

lpc$(EXEEXT):	lpc.o ../cups/$(LIBCUPS)
	echo Linking $@...
	$(CC) $(LDFLAGS) -o $@ lpc.o $(LIBS)


#
# lpq
#

lpq$(EXEEXT):	lpq.o ../cups/$(LIBCUPS)
	echo Linking $@...
	$(CC) $(LDFLAGS) -o $@ lpq.o $(LIBS)


#
# lpr
#

lpr$(EXEEXT):	lpr.o ../cups/$(LIBCUPS)
	echo Linking $@...
	$(CC) $(LDFLAGS) -o $@ lpr.o $(LIBS)


#
# lprm
#

lprm$(EXEEXT):	lprm.o ../cups/$(LIBCUPS)
	echo Linking $@...
	$(CC) $(LDFLAGS) -o $@ lprm.o $(LIBS)


#
# Dependencies...
#

include Dependencies


#
# End of "$Id: Makefile 10996 2013-05-29 11:51:34Z msweet $".
#
