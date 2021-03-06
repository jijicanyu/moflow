include ../Makefile.config
include $(PIQI_ROOT)/make/Makefile.ocaml
include $(PIQI_ROOT)/piqilib/Makefile.piqi_common


RESULT = piqilib
PIQI_LIB_NAME = lib
LIBINSTALL_FILES = \
	$(RESULT).cmxa \
	$(RESULT).$(EXT_LIB) $(RES_CLIB) \
	$(shell ls *.cmi 2>/dev/null) \
	piqirun_ext.mli

# we're not building byte-code library by default, only when requested
# explicitly, e.g. from top-level Makefile
#ifneq ($(findstring bcl,$(MAKECMDGOALS)),)
LIBINSTALL_FILES += $(RESULT).cma
#endif

ifneq ($(findstring debug,$(MAKECMDGOALS)),)
LIBINSTALL_FILES += $(RESULT).cma
endif


PACKS += xmlm

STATIC = 1 # force creation of static library (don't create dlls)

PIQI_SOURCES_LIB = \
	piqi_json_parser.mll \
	piqi_json_gen.ml \
	piqi_json.ml \
	\
	piqi_base64.ml \
	piqobj_to_json.ml \
	piqobj_of_json.ml \
	\
	piqi_xml.ml \
	piqobj_to_xml.ml \
	piqobj_of_xml.ml \
	\
	piq.ml \
	piqi_convert.ml \
	\
	piqirun_ext.mli \
	piqirun_ext.ml \
	\
	piqi_light.ml \
	piqi_getopt.ml \


PIQI_SOURCES_TAIL += $(PIQI_SOURCES_LIB)


PIQI_PIQI = piqi-lang.piqi piqi-impl.piqi


PIQICC = ../piqicc/piqicc
PIQICC_FLAGS = -I $(PIQI_ROOT)


all: ncl dcl


debug: dcl install


post_target:: install


piqi_lang_piqi.ml: $(PIQI_PIQI)
	$(PIQICC) $(PIQICC_FLAGS) -o $@.tmp.ml \
		--spec piqi.piqi --lang piqi-lang.piqi --impl piqi-impl.piqi
	$(CAMLP4_O) -o $@ $@.tmp.ml


#NOTE: when installing, uninstall first to avoid "already installed" error
install: uninstall libinstall
	$(PIQI_ROOT)/make/piqi-fix-libinstall $(RESULT) $(PIQI_LIB_NAME)

uninstall: libuninstall


clean::
	rm -f piqi_lang_piqi.ml.tmp.ml


include $(OCAMLMAKEFILE)
