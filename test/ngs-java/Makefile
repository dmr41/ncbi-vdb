# ===========================================================================
#
#                            PUBLIC DOMAIN NOTICE
#               National Center for Biotechnology Information
#
#  This software/database is a "United States Government Work" under the
#  terms of the United States Copyright Act.  It was written as part of
#  the author's official duties as a United States Government employee and
#  thus cannot be copyrighted.  This software/database is freely available
#  to the public for use. The National Library of Medicine and the U.S.
#  Government have not placed any restriction on its use or reproduction.
#
#  Although all reasonable efforts have been taken to ensure the accuracy
#  and reliability of the software and data, the NLM and the U.S.
#  Government do not and cannot warrant the performance or results that
#  may be obtained by using this software or data. The NLM and the U.S.
#  Government disclaim all warranties, express or implied, including
#  warranties of performance, merchantability or fitness for any particular
#  purpose.
#
#  Please cite the author in any work or product based on this material.
#
# ===========================================================================


default: runtests

TOP ?= $(abspath ../..)

MODULE = test/ngs-java

JARS = \
	ngs_jtest

ALL_TARGS = \
	$(JARS)

include $(TOP)/build/Makefile.env

ifeq (,$(NGS_JAVA_JAR))

$(ALL_TARGS):
	@true 
	
runtests:
	@ echo "NGS Java tests are not run since ngs-java library does not appear to be installed"

else
ifeq (,$(NGS_LIBDIR))

$(ALL_TARGS):
	@true 
	
runtests:
	@ echo "NGS Java tests are not run since ngs-sdk library does not appear to be installed"

else

JUNIT_JAR = $(SRCDIR)/../junit-4.11.jar

CLASSPATH = $(NGS_JAVA_JAR):$(JUNIT_JAR)

$(JARS): makejdirs
	@ $(MAKE_CMD) $(TEST_CLSPATH)/$@

.PHONY: $(JARS)

clean: stdclean

%.class: %.java
	javac $^ -d $(CLASSDIR) -classpath $(CLASSPATH)
    
#-------------------------------------------------------------------------------
# tests for ncbi-ngs
#
NGS_SRC =                      \
	ngs_test_CSRA1
    
NGS_CLS = \
	$(addsuffix .class,$(NGS_SRC))

# use standard java System.loadLibrary
VARS += -Dvdb.System.loadLibrary=1

# remove all found JNI DLL-s
#VARS += -Dvdb.deleteLibraries=1

# custom load library search
#VARS += -Dvdb.loadLibraryLocations=W

#VARS += -Dvdb.log=FINEST

#J=/net/pan1/sra-test/local/jdk1.7.0_51/bin/

$(TEST_CLSPATH)/ngs_jtest: $(NGS_CLS)

.PHONY: $(TEST_CLSPATH)/ngs_jtest

#-------------------------------------------------------------------------------
# runtests
#

runtests: $(JARS) test

JAVABITS=-d$(BITS)

test: $(NGS_CLS)
	@ echo Run $(CLASSDIR)/$(NGS_SRC)
	@ echo java $(JAVABITS) -cp $(CLASSDIR):$(CLASSPATH) -Dvdb.System.loadLibrary=1 -Djava.library.path=$(NGS_LIBDIR):$(LIBDIR) org.junit.runner.JUnitCore $(NGS_SRC)
	java $(JAVABITS) -cp $(CLASSDIR):$(CLASSPATH) -Dvdb.System.loadLibrary=1 -Djava.library.path=$(NGS_LIBDIR):$(LIBDIR) org.junit.runner.JUnitCore $(NGS_SRC)  

endif    
endif
