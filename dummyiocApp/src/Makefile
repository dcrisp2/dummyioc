TOP=../..

include $(TOP)/configure/CONFIG
#----------------------------------------
#  ADD MACRO DEFINITIONS AFTER THIS LINE

#=============================
# Build the IOC support library

LIBRARY_IOC += dummyiocSupport

# xxxRecord.h will be created from xxxRecord.dbd
DBDINC += xxxRecord
# Install devXxxSoft.dbd into <top>/dbd
DBD += xxxSupport.dbd

# Compile and add the code to the support library
dummyiocSupport_SRCS += xxxRecord.c
dummyiocSupport_SRCS += devXxxSoft.c

# Link locally-provided code into the support library,
# rather than directly into the IOC application.
dummyiocSupport_SRCS += dbSubExample.c
dummyiocSupport_SRCS += dummyiocHello.c
dummyiocSupport_SRCS += initTrace.c

dummyiocSupport_LIBS += $(EPICS_BASE_IOC_LIBS)

#=============================
# Build the IOC application

PROD_IOC = dummyioc
# dummyioc.dbd will be created and installed
DBD += dummyioc.dbd

# dummyioc.dbd will be made up from these files:
dummyioc_DBD += base.dbd
dummyioc_DBD += xxxSupport.dbd
dummyioc_DBD += dbSubExample.dbd
dummyioc_DBD += dummyiocHello.dbd
dummyioc_DBD += initTrace.dbd

# dummyioc_registerRecordDeviceDriver.cpp derives from dummyioc.dbd
dummyioc_SRCS += dummyioc_registerRecordDeviceDriver.cpp

# Build the main IOC entry point on workstation OSs.
dummyioc_SRCS_DEFAULT += dummyiocMain.cpp
dummyioc_SRCS_vxWorks += -nil-

# Add support from base/src/vxWorks if needed
#dummyioc_OBJS_vxWorks += $(EPICS_BASE_BIN)/vxComLibrary

# Link in the code from the support library
dummyioc_LIBS += dummyiocSupport

# NOTE: To build SNL programs, SNCSEQ must be defined
# in the <top>/configure/RELEASE file

ifneq ($(SNCSEQ),)
    # Build sncExample into dummyiocSupport
    sncExample_SNCFLAGS += +r
    dummyioc_DBD += sncExample.dbd
    dummyiocSupport_SRCS += sncExample.stt
    dummyiocSupport_LIBS += seq pv
    dummyioc_LIBS += seq pv

    # Build sncProgram as a standalone program
    PROD_HOST += sncProgram
    sncProgram_SNCFLAGS += +m
    sncProgram_SRCS += sncProgram.st
    sncProgram_LIBS += seq pv
    sncProgram_LIBS += $(EPICS_BASE_HOST_LIBS)
endif

# Finally link to the EPICS Base libraries
dummyioc_LIBS += $(EPICS_BASE_IOC_LIBS)

#=============================

include $(TOP)/configure/RULES
#----------------------------------------
#  ADD RULES AFTER THIS LINE

