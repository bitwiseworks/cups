#
# "$Id: Makefile 11776 2014-03-28 19:16:05Z msweet $"
#
# System V commands makefile for CUPS.
#
# Copyright 2007-2014 by Apple Inc.
# Copyright 1997-2006 by Easy Software Products, all rights reserved.
#
# These coded instructions, statements, and computer programs are the
# property of Apple Inc. and are protected by Federal copyright
# law.  Distribution and use rights are outlined in the file "LICENSE.txt"
# which should have been included with this file.  If this file is
# file is missing or damaged, see the license at "http://www.cups.org/".
#

include ../Makedefs

TARGETS	=	cancel$(EXEEXT) cupsaccept$(EXEEXT) cupsaddsmb$(EXEEXT) cupsctl$(EXEEXT) cupstestdsc$(EXEEXT) cupstestppd$(EXEEXT) \
		lp$(EXEEXT) lpadmin$(EXEEXT) lpinfo$(EXEEXT) lpmove$(EXEEXT) lpoptions$(EXEEXT) lpstat$(EXEEXT)
OBJS	=	cancel.o cupsaccept.o cupsaddsmb.o cupsctl.o cupstestdsc.o \
		cupstestppd.o lp.o lpadmin.o lpinfo.o lpmove.o lpoptions.o \
		lpstat.o


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
	$(RM) accept cupsdisable cupsenable cupsreject reject


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
	echo Installing System V admin printing commands in $(SBINDIR)
	$(INSTALL_DIR) -m 755 $(SBINDIR)
	$(INSTALL_BIN) cupsaccept$(EXEEXT) $(SBINDIR)
	$(INSTALL_BIN) cupsaddsmb$(EXEEXT) $(SBINDIR)
	$(INSTALL_BIN) cupsctl$(EXEEXT) $(SBINDIR)
	$(INSTALL_BIN) lpadmin$(EXEEXT) $(SBINDIR)
	$(INSTALL_BIN) lpinfo$(EXEEXT) $(SBINDIR)
	$(INSTALL_BIN) lpmove$(EXEEXT) $(SBINDIR)
	$(RM) $(SBINDIR)/accept$(EXEEXT)
	$(LN) cupsaccept$(EXEEXT) $(SBINDIR)/accept
	$(RM) $(SBINDIR)/cupsdisable$(EXEEXT)
	$(LN) cupsaccept$(EXEEXT) $(SBINDIR)/cupsdisable
	$(RM) $(SBINDIR)/cupsenable$(EXEEXT)
	$(LN) cupsaccept$(EXEEXT) $(SBINDIR)/cupsenable
	$(RM) $(SBINDIR)/cupsreject$(EXEEXT)
	$(LN) cupsaccept$(EXEEXT) $(SBINDIR)/cupsreject
	$(RM) $(SBINDIR)/reject$(EXEEXT)
	$(LN) cupsaccept$(EXEEXT) $(SBINDIR)/reject
	echo Installing System V user printing commands in $(BINDIR)
	$(INSTALL_DIR) -m 755 $(BINDIR)
	$(INSTALL_BIN) cancel$(EXEEXT) $(BINDIR)
	$(INSTALL_BIN) cupstestdsc$(EXEEXT) $(BINDIR)
	$(INSTALL_BIN) cupstestppd$(EXEEXT) $(BINDIR)
	$(INSTALL_BIN) lp$(EXEEXT) $(BINDIR)
	$(INSTALL_BIN) lpoptions$(EXEEXT) $(BINDIR)
	$(INSTALL_BIN) lpstat$(EXEEXT) $(BINDIR)
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
	$(RM) $(BINDIR)/cancel$(EXEEXT)
	$(RM) $(BINDIR)/cupstestdsc$(EXEEXT)
	$(RM) $(BINDIR)/cupstestppd$(EXEEXT)
	$(RM) $(BINDIR)/lp$(EXEEXT)
	$(RM) $(BINDIR)/lpoptions$(EXEEXT)
	$(RM) $(BINDIR)/lpstat$(EXEEXT)
	-$(RMDIR) $(BINDIR)
	$(RM) $(SBINDIR)/accept$(EXEEXT)
	$(RM) $(SBINDIR)/cupsaccept$(EXEEXT)
	$(RM) $(SBINDIR)/cupsaddsmb$(EXEEXT)
	$(RM) $(SBINDIR)/cupsaccept$(EXEEXT)
	$(RM) $(SBINDIR)/cupsdisable$(EXEEXT)
	$(RM) $(SBINDIR)/cupsenable$(EXEEXT)
	$(RM) $(SBINDIR)/cupsreject$(EXEEXT)
	$(RM) $(SBINDIR)/lpadmin$(EXEEXT)
	$(RM) $(SBINDIR)/lpinfo$(EXEEXT)
	$(RM) $(SBINDIR)/lpmove$(EXEEXT)
	$(RM) $(SBINDIR)/reject$(EXEEXT)
	-$(RMDIR) $(SBINDIR)


#
# cancel
#

cancel$(EXEEXT):	cancel.o ../cups/$(LIBCUPS)
	echo Linking $@...
	$(CC) $(LDFLAGS) -o $@ cancel.o $(LIBS)


#
# cupsaccept
#

cupsaccept$(EXEEXT):	cupsaccept.o ../cups/$(LIBCUPS)
	echo Linking $@...
	$(CC) $(LDFLAGS) -o $@ cupsaccept.o $(LIBS)
	for file in accept cupsenable cupsdisable cupsreject reject; do \
		$(RM) $$file; \
		$(LN) $@ $$file; \
	done


#
# cupsaddsmb
#

cupsaddsmb$(EXEEXT):	cupsaddsmb.o ../cups/$(LIBCUPS)
	echo Linking $@...
	$(CC) $(LDFLAGS) -o $@ cupsaddsmb.o $(LIBS)


#
# cupsctl
#

cupsctl$(EXEEXT):	cupsctl.o ../cups/$(LIBCUPS)
	echo Linking $@...
	$(CC) $(LDFLAGS) -o $@ cupsctl.o $(LIBS)


#
# cupstestdsc
#

cupstestdsc$(EXEEXT):	cupstestdsc.o ../cups/$(LIBCUPS)
	echo Linking $@...
	$(CC) $(LDFLAGS) -o $@ cupstestdsc.o $(LIBS)


#
# cupstestppd
#

cupstestppd$(EXEEXT):	cupstestppd.o ../cups/$(LIBCUPS) ../filter/$(LIBCUPSIMAGE)
	echo Linking $@...
	$(CC) $(LDFLAGS) -o $@ cupstestppd.o $(LINKCUPSIMAGE) $(IMGLIBS) $(LIBS)

cupstestppd-static$(EXEEXT):	cupstestppd.o ../cups/$(LIBCUPSSTATIC) ../filter/libcupsimage.a
	echo Linking $@...
	$(CC) $(LDFLAGS) -o $@ cupstestppd.o ../filter/libcupsimage.a \
		../cups/$(LIBCUPSSTATIC) $(IMGLIBS) $(LIBGSSAPI) $(LIBS) $(LIBZ)


#
# lp
#

lp$(EXEEXT):	lp.o ../cups/$(LIBCUPS)
	echo Linking $@...
	$(CC) $(LDFLAGS) -o $@ lp.o $(LIBS)


#
# lpadmin
#

lpadmin$(EXEEXT):	lpadmin.o ../cups/$(LIBCUPS)
	echo Linking $@...
	$(CC) $(LDFLAGS) -o $@ lpadmin.o $(LIBZ) $(LIBS)


#
# lpinfo
#

lpinfo$(EXEEXT):	lpinfo.o ../cups/$(LIBCUPS)
	echo Linking $@...
	$(CC) $(LDFLAGS) -o $@ lpinfo.o $(LIBS)


#
# lpmove
#

lpmove$(EXEEXT):	lpmove.o ../cups/$(LIBCUPS)
	echo Linking $@...
	$(CC) $(LDFLAGS) -o $@ lpmove.o $(LIBS)


#
# lpoptions
#

lpoptions$(EXEEXT):	lpoptions.o ../cups/$(LIBCUPS)
	echo Linking $@...
	$(CC) $(LDFLAGS) -o $@ lpoptions.o $(LIBZ) $(LIBS)


#
# lpstat
#

lpstat$(EXEEXT):	lpstat.o ../cups/$(LIBCUPS)
	echo Linking $@...
	$(CC) $(LDFLAGS) -o $@ lpstat.o $(LIBS)


#
# Dependencies...
#

include Dependencies


#
# End of "$Id: Makefile 11776 2014-03-28 19:16:05Z msweet $".
#
