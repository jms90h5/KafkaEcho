# NOTE: This makefile is designed to be run from one level up in the directory
# hierarchy compared to where it resides. For instance, if this makefile is
# located at 'output/build/Makefile', then it can be launched as:
# 'make -f build/Makefile' from the 'output' directory.

SHELL=/bin/bash
ifeq ($(CXX),)
  CXX=g++
endif
ifeq ($(CXX_LN),)
  CXX_LN=g++
endif
PKGCONFIG := /opt/ibm/InfoSphere_Streams/4.3.1.1/bin/dst-pe-pkg-config-alternative.sh
SPL_BUNDLE_BUILDER := /opt/ibm/InfoSphere_Streams/4.3.1.1/system/impl/bin/spl-bundle-builder
SO_INCLUDE := $(shell $(PKGCONFIG) --cflags dst-spl-pe-install)
SO_LIB := $(shell $(PKGCONFIG) --libs dst-spl-pe-install)
SO_STAND_LIB := $(shell $(PKGCONFIG) --libs dst-spl-pe-install-standalone)
SPL_CXXFLAGS = -Wall -Wno-unused -Wno-sign-compare
SPL_CXXFLAGS += -D_REENTRANT -fPIC -finline-functions -fmessage-length=0
SPL_CXXFLAGS += $(SO_INCLUDE) -O3 -DNDEBUG  -std=gnu++98 
SPL_LDFLAGS = $(SO_LIB)

SPL_TYPES = build/type/BeJwrMSo2zk6tLDbPTS0uTkxPBQAysQYl.o 
SPL_FUNCTIONS = 
SPL_OPERATORS = build/operator/KafkaConsumer_1.o \
                build/operator/Custom_2.o \
                build/operator/KafkaProducer_3.o 
SPL_SO_BINS = bin/KafkaConsumer_1.so \
              bin/Custom_2.so \
              bin/KafkaProducer_3.so 

SPL_ADL_FILE = application.KafkaEcho.adl

SPL_OUTPUT_DIR_ROOT = $(shell printf '%q' '/home/streamsadmin/workspace/KafkaEcho/output/application.KafkaEcho')
SPL_OUTPUT_DIR = $(shell printf '%q' 'BuildConfig')

.PHONY: distclean clean all types functions operators sos bundle toolkits standalone sablink

all: bin types functions operators sos standalone bundle sablink

distclean clean:
	@rm -fr bin/* build/type build/function build/operator build/bundle build/standalone application.KafkaEcho.sab

bin:
	@mkdir -m 755 bin

types: $(SPL_TYPES)

build/type/BeJwrMSo2zk6tLDbPTS0uTkxPBQAysQYl.o: \
	    src/type/BeJwrMSo2zk6tLDbPTS0uTkxPBQAysQYl.h \
	    src/type/BeJwrMSo2zk6tLDbPTS0uTkxPBQAysQYl.cpp \
	    build/cppOptions \
	    | build/type
	@echo ' [CXX-type] tuple<rstring key,rstring message>'
	@$(CXX) -o $@ -c $(SPL_CXXFLAGS) src/type/BeJwrMSo2zk6tLDbPTS0uTkxPBQAysQYl.cpp

build/type:
	-@mkdir -m 755 build/type

operators: $(SPL_OPERATORS)

-include build/operator/KafkaConsumer_1.d
build/operator/KafkaConsumer_1.o: \
	    src/operator/KafkaConsumer_1.h \
	    src/operator/KafkaConsumer_1.cpp\
	    build/dep/operator/KafkaConsumer_1.dep  \
	    build/cppOptions \
	    | build/operator
	@echo ' [CXX-operator] KafkaConsumer_1'
	@$(CXX) -o $@ -MD -I /opt/ibm/InfoSphere_Streams/4.3.1.1/toolkits/com.ibm.streamsx.kafka/impl/nl/include -I /opt/ibm/InfoSphere_Streams/4.3.1.1/toolkits/spl/spl/utility/JavaOp/../../../impl/include -I /opt/ibm/InfoSphere_Streams/4.3.1.1/java/include -DSPLJAVAFUNCTION_ADL_FILENAME=\"application.KafkaEcho.adl\" -c $(SPL_CXXFLAGS) src/operator/KafkaConsumer_1.cpp

-include build/operator/Custom_2.d
build/operator/Custom_2.o: \
	    src/operator/Custom_2.h \
	    src/operator/Custom_2.cpp\
	    build/dep/operator/Custom_2.dep  \
	    build/cppOptions \
	    | build/operator
	@echo ' [CXX-operator] Custom_2'
	@$(CXX) -o $@ -MD -I /opt/ibm/InfoSphere_Streams/4.3.1.1/toolkits/spl/impl/nl/include -DSPLJAVAFUNCTION_ADL_FILENAME=\"application.KafkaEcho.adl\" -c $(SPL_CXXFLAGS) src/operator/Custom_2.cpp

-include build/operator/KafkaProducer_3.d
build/operator/KafkaProducer_3.o: \
	    src/operator/KafkaProducer_3.h \
	    src/operator/KafkaProducer_3.cpp\
	    build/dep/operator/KafkaProducer_3.dep  \
	    build/cppOptions \
	    | build/operator
	@echo ' [CXX-operator] KafkaProducer_3'
	@$(CXX) -o $@ -MD -I /opt/ibm/InfoSphere_Streams/4.3.1.1/toolkits/com.ibm.streamsx.kafka/impl/nl/include -I /opt/ibm/InfoSphere_Streams/4.3.1.1/toolkits/spl/spl/utility/JavaOp/../../../impl/include -I /opt/ibm/InfoSphere_Streams/4.3.1.1/java/include -DSPLJAVAFUNCTION_ADL_FILENAME=\"application.KafkaEcho.adl\" -c $(SPL_CXXFLAGS) src/operator/KafkaProducer_3.cpp

build/operator:
	-@mkdir -m 755 -p build/operator

functions: $(SPL_FUNCTIONS)

sos: $(SPL_SO_BINS)

SOKafkaConsumer_1 = \
	    build/operator/KafkaConsumer_1.o \
	    build/type/BeJwrMSo2zk6tLDbPTS0uTkxPBQAysQYl.o 

bin/KafkaConsumer_1.so: \
	$(SOKafkaConsumer_1) \
	build/dep/KafkaConsumer_1.so \
	build/ldOptions
	@echo ' [LD-so] so KafkaConsumer_1'
	@$(CXX_LN) $(SPL_CXXFLAGS) -o bin/KafkaConsumer_1.so -fPIC -shared $(SOKafkaConsumer_1) -lstreams-stdtk-javaop -L /opt/ibm/InfoSphere_Streams/4.3.1.1/java/jre/lib/amd64/j9vm -Wl,-rpath -Wl,/opt/ibm/InfoSphere_Streams/4.3.1.1/java/jre/lib/amd64/j9vm -lpthread -ldl -lstreams-stdtk-runtime $(SPL_LDFLAGS)

SOCustom_2 = \
	    build/operator/Custom_2.o \
	    build/type/BeJwrMSo2zk6tLDbPTS0uTkxPBQAysQYl.o 

bin/Custom_2.so: \
	$(SOCustom_2) \
	build/dep/Custom_2.so \
	build/ldOptions
	@echo ' [LD-so] so Custom_2'
	@$(CXX_LN) $(SPL_CXXFLAGS) -o bin/Custom_2.so -fPIC -shared $(SOCustom_2) $(SPL_LDFLAGS)

SOKafkaProducer_3 = \
	    build/operator/KafkaProducer_3.o \
	    build/type/BeJwrMSo2zk6tLDbPTS0uTkxPBQAysQYl.o 

bin/KafkaProducer_3.so: \
	$(SOKafkaProducer_3) \
	build/dep/KafkaProducer_3.so \
	build/ldOptions
	@echo ' [LD-so] so KafkaProducer_3'
	@$(CXX_LN) $(SPL_CXXFLAGS) -o bin/KafkaProducer_3.so -fPIC -shared $(SOKafkaProducer_3) -lstreams-stdtk-javaop -L /opt/ibm/InfoSphere_Streams/4.3.1.1/java/jre/lib/amd64/j9vm -Wl,-rpath -Wl,/opt/ibm/InfoSphere_Streams/4.3.1.1/java/jre/lib/amd64/j9vm -lpthread -ldl -lstreams-stdtk-runtime $(SPL_LDFLAGS)

standalone: bin/standalone.exe

build/standalone/standalone.o: \
	    src/standalone/standalone.h \
	    src/standalone/standalone.cpp  \
	    build/cppOptions \
	    | build/standalone
	@echo ' [CXX-standalone] standalone'
	@$(CXX) -o $@ -c $(SPL_CXXFLAGS) src/standalone/standalone.cpp

bin/standalone.exe: build/standalone/standalone.o build/ldOptions
	@echo ' [LD-standalone] standalone'
	@$(CXX_LN) $(SPL_CXXFLAGS) -o bin/standalone.exe build/standalone/standalone.o $(SO_STAND_LIB)
	@echo ' [LN-standalone] standalone '
	@ln -sf standalone bin/application.KafkaEcho
build/standalone:
	-@mkdir -m 755 build/standalone

sablink:

	@ln -sf ../application.KafkaEcho.sab bin/sab

bundle: application.KafkaEcho.sab

-include build/dep/application.KafkaEcho.sab.d
application.KafkaEcho.sab: $(SPL_SO_BINS) $(SPL_ADL_FILE) build/dep/application.KafkaEcho.sab.d bin/standalone.exe
	@echo ' [Bundle] application.KafkaEcho.sab'
	@$(SPL_BUNDLE_BUILDER) $(SPL_OUTPUT_DIR_ROOT) $(SPL_OUTPUT_DIR) $(SPL_ADL_FILE) application.KafkaEcho.sab

