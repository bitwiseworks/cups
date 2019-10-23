#
# Backend makefile for CUPS.
#
# Copyright 2007-2017 by Apple Inc.
# Copyright 1997-2007 by Easy Software Products, all rights reserved.
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

#
# Object files...
#

# RBACKENDS are installed mode 0700 so cupsd will run them as root...
#
# UBACKENDS and ULBACKENDS are installed mode 0755 so cupsd will run them as
# an unprivileged user...
#
# See http://www.cups.org/doc/api-filter.html for more info...
RBACKENDS =	\
		ipp$(EXEEXT) \
		lpd$(EXEEXT) \
		$(DNSSD_BACKEND)
UBACKENDS =	\
		snmp$(EXEEXT) \
		socket$(EXEEXT)
ULBACKENDS =	\
		usb$(EXEEXT)
UNITTESTS =	\
		test1284$(EXEEXT) \
		testbackend$(EXEEXT) \
		testsupplies$(EXEEXT)
TARGETS =	\
		libbackend.a \
		$(RBACKENDS) \
		$(UBACKENDS) \
		$(ULBACKENDS)
LIBOBJS	=	\
		ieee1284.o \
		network.o \
		runloop.o \
		snmp-supplies.o
OBJS	=	\
		ipp.o \
		lpd.o \
		dnssd.o \
		snmp.o \
		socket.o \
		test1284.o \
		testbackend.o \
		testsupplies.o \
		usb.o


#
# Make all targets...
#

all:	$(TARGETS)


#
# Make library targets...
#

libs:	$(ULBACKENDS)


#
# Make unit tests...
#

unittests:	$(UNITTESTS)


#
# Clean all object files...
#

clean:
	$(RM) $(OBJS) $(TARGETS) $(UNITTESTS) $(LIBOBJS) http https ipps mdns socket-static


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
	if test "x$(USBQUIRKS)" != x; then \
		echo Installing USB quirks in $(USBQUIRKS); \
		$(INSTALL_DIR) -m 755 $(USBQUIRKS); \
		$(INSTALL_DATA) org.cups.usb-quirks $(USBQUIRKS); \
	fi


#
# Install programs...
#

install-exec:	$(INSTALLXPC)
	echo Installing backends in $(SERVERBIN)/backend
	$(INSTALL_DIR) -m 755 $(SERVERBIN)/backend
	for file in $(RBACKENDS); do \
		$(LIBTOOL) $(INSTALL_BIN) -m 700 $$file $(SERVERBIN)/backend; \
	done
	for file in $(UBACKENDS); do \
		$(INSTALL_BIN) $$file $(SERVERBIN)/backend; \
	done
	for file in $(IPPALIASES); do \
		$(RM) $(SERVERBIN)/backend/$$file; \
		$(LN) ipp$(EXEEXT) $(SERVERBIN)/backend/$$file; \
	done
	if test "x$(DNSSD_BACKEND)" != x -a `uname` = Darwin; then \
		$(RM) $(SERVERBIN)/backend/mdns; \
		$(LN) $(DNSSD_BACKEND) $(SERVERBIN)/backend/mdns; \
	fi
	if test "x$(SYMROOT)" != "x"; then \
		$(INSTALL_DIR) $(SYMROOT); \
		for file in $(RBACKENDS) $(UBACKENDS); do \
			cp $$file $(SYMROOT); \
			dsymutil $(SYMROOT)/$$file; \
		done \
	fi

install-xpc:	ipp
	echo Installing XPC backends in $(SERVERBIN)/apple
	$(INSTALL_DIR) -m 755 $(SERVERBIN)/apple
	$(LIBTOOL) $(INSTALL_BIN) ipp $(SERVERBIN)/apple
	for file in $(IPPALIASES); do \
		$(RM) $(SERVERBIN)/apple/$$file; \
		$(LN) ipp$(EXEEXT) $(SERVERBIN)/apple/$$file; \
	done


#
# Install headers...
#

install-headers:


#
# Install libraries...
#

install-libs:
	echo Installing backends in $(SERVERBIN)/backend
	$(INSTALL_DIR) -m 755 $(SERVERBIN)/backend
	for file in $(ULBACKENDS); do \
		$(INSTALL_BIN) $$file $(SERVERBIN)/backend; \
	done
	if test "x$(SYMROOT)" != "x"; then \
		$(INSTALL_DIR) $(SYMROOT); \
		for file in $(ULBACKENDS); do \
			cp $$file $(SYMROOT); \
			dsymutil $(SYMROOT)/$$file; \
		done \
	fi


#
# Uninstall all targets...
#

uninstall:
	$(RM) $(SERVERBIN)/apple/ipp
	for file in $(IPPALIASES); do \
		$(RM) $(SERVERBIN)/apple/$$file; \
	done
	-$(RMDIR) $(SERVERBIN)/apple
	for file in $(RBACKENDS) $(UBACKENDS) $(ULBACKENDS); do \
		$(RM) $(SERVERBIN)/backend/$$file; \
	done
	for file in $(IPPALIASES); do \
		$(RM) $(SERVERBIN)/backend/$$file; \
	done
	-$(RMDIR) $(SERVERBIN)/backend
	-$(RMDIR) $(SERVERBIN)


#
# test1284
#

test1284$(EXEEXT):	test1284.o ../cups/$(LIBCUPSSTATIC)
	echo Linking $@...
	$(LD_CC) $(LDFLAGS) -o $@ test1284.o ../cups/$(LIBCUPSSTATIC) \
		$(LIBGSSAPI) $(SSLLIBS) $(DNSSDLIBS) $(COMMONLIBS) $(LIBZ)


#
# testbackend
#

testbackend$(EXEEXT):	testbackend.o ../cups/$(LIBCUPSSTATIC)
	echo Linking $@...
	$(LD_CC) $(LDFLAGS) -o $@ testbackend.o ../cups/$(LIBCUPSSTATIC) \
		$(LIBGSSAPI) $(SSLLIBS) $(DNSSDLIBS) $(COMMONLIBS) $(LIBZ)


#
# testsupplies
#

testsupplies$(EXEEXT):	testsupplies.o libbackend.a ../cups/$(LIBCUPSSTATIC)
	echo Linking $@...
	$(LD_CC) $(LDFLAGS) -o $@ testsupplies.o libbackend.a \
		../cups/$(LIBCUPSSTATIC) $(LIBGSSAPI) $(SSLLIBS) $(DNSSDLIBS) \
		$(COMMONLIBS) $(LIBZ)


#
# libbackend.a
#

libbackend.a:	$(LIBOBJS)
	echo Archiving $@...
	$(RM) $@
	$(AR) $(ARFLAGS) $@ $(LIBOBJS)
	$(RANLIB) $@


#
# dnssd
#

dnssd$(EXEEXT):	dnssd.o ../cups/$(LIBCUPS) libbackend.a
	echo Linking $@...
	$(LD_CC) $(LDFLAGS) -o $@ dnssd.o libbackend.a $(LIBS)
	$(CODE_SIGN) -s "$(CODE_SIGN_IDENTITY)" $@
	if test `uname` = Darwin; then \
		$(RM) mdns; \
		$(LN) dnssd mdns; \
	fi


#
# ipp
#

ipp$(EXEEXT):	ipp.o ../cups/$(LIBCUPS) libbackend.a
	echo Linking $@...
	$(LD_CC) $(LDFLAGS) -o $@ ipp.o libbackend.a $(LIBS)
	$(CODE_SIGN) -s "$(CODE_SIGN_IDENTITY)" $@
	$(RM) http
	$(LN) ipp$(EXEEXT) http


#
# lpd
#

lpd$(EXEEXT):	lpd.o ../cups/$(LIBCUPS) libbackend.a
	echo Linking $@...
	$(LD_CC) $(LDFLAGS) -o $@ lpd.o libbackend.a $(LIBS)
	$(CODE_SIGN) -s "$(CODE_SIGN_IDENTITY)" $@


#
# snmp
#

snmp$(EXEEXT):	snmp.o ../cups/$(LIBCUPS) libbackend.a
	echo Linking $@...
	$(LD_CC) $(LDFLAGS) -o $@ snmp.o libbackend.a $(LIBS)
	$(CODE_SIGN) -s "$(CODE_SIGN_IDENTITY)" $@


#
# socket
#

socket$(EXEEXT):	socket.o ../cups/$(LIBCUPS) libbackend.a
	echo Linking $@...
	$(LD_CC) $(LDFLAGS) -o $@ socket.o libbackend.a $(LIBS)
	$(CODE_SIGN) -s "$(CODE_SIGN_IDENTITY)" $@


#
# usb
#

usb$(EXEEXT):	usb.o ../cups/$(LIBCUPS) libbackend.a
	echo Linking $@...
	$(LD_CC) $(ARCHFLAGS) $(LDFLAGS) -o $@ usb.o libbackend.a $(LIBUSB) \
		$(BACKLIBS) $(LIBS)
	$(CODE_SIGN) -s "$(CODE_SIGN_IDENTITY)" $@
usb.o:	usb.c usb-darwin.c usb-libusb.c usb-unix.c


#
# Dependencies...
#

include Dependencies
