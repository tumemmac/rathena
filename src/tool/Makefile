
COMMON_OBJ = minicore.o malloc.o showmsg.o strlib.o utils.o des.o grfio.o
COMMON_DIR_OBJ = $(COMMON_OBJ:%=../common/obj/%)
COMMON_H = $(shell ls ../common/*.hpp)
COMMON_INCLUDE = -I../common/

LIBCONFIG_H = $(shell ls ../../3rdparty/libconfig/*.h)
LIBCONFIG_AR = ../../3rdparty/libconfig/obj/libconfig.a
LIBCONFIG_INCLUDE = -I../../3rdparty/libconfig

YAML_CPP_OBJ = $(shell find ../../3rdparty/yaml-cpp/ -type f -name "*.cpp" | sed -e "s/\.cpp/\.o/g" )
YAML_CPP_DIR_OBJ = $(YAML_CPP_OBJ:%=obj/%)
YAML_CPP_AR = ../../3rdparty/yaml-cpp/obj/yaml-cpp.a
YAML_CPP_H = $(shell find ../../3rdparty/yaml-cpp/ -type f -name "*.h")
YAML_CPP_INCLUDE = -I../../3rdparty/yaml-cpp/include

OTHER_H = ../config/renewal.hpp

MAPCACHE_OBJ = obj_all/mapcache.o

CSV2YAML_OBJ = obj_all/csv2yaml.o



#####################################################################
.PHONY : all mapcache csv2yaml clean help

all: mapcache csv2yaml

mapcache: obj_all $(MAPCACHE_OBJ) $(COMMON_DIR_OBJ) $(LIBCONFIG_OBJ)
	@echo "	LD	$@"
	@g++  -L/usr/lib -o ../../mapcache $(MAPCACHE_OBJ) $(COMMON_DIR_OBJ) $(LIBCONFIG_AR) -lpcre -lpthread -lz  -ldl

csv2yaml: obj_all $(CSV2YAML_OBJ) $(COMMON_DIR_OBJ) $(YAML_CPP_AR)
	@echo "	LD	$@"
	@g++  -L/usr/lib -o ../../csv2yaml $(CSV2YAML_OBJ) $(COMMON_DIR_OBJ) $(YAML_CPP_AR) -lpcre -lpthread -lz  -ldl

clean:
	@echo "	CLEAN	tool"
	@rm -rf obj_all/*.o ../../mapcache

help:
	@echo "possible targets are 'mapcache' 'all' 'clean' 'help'"
	@echo "'mapcache'  - mapcache generator"
	@echo "'csv2yaml'  - csv2yaml converter"
	@echo "'all'       - builds all above targets"
	@echo "'clean'     - cleans builds and objects"
	@echo "'help'      - outputs this message"

#####################################################################

obj_all:
	-mkdir obj_all

obj_all/%.o: %.cpp $(COMMON_H) $(OTHER_H) $(LIBCONFIG_H) $(YAML_CPP_H)
	@echo "	CXX	$<"
	@g++ -g -O2 -std=c++11 $(COMMON_INCLUDE) $(LIBCONFIG_INCLUDE) $(YAML_CPP_INCLUDE)  -I../common -DHAS_TLS -fno-strict-aliasing -DHAVE_SETRLIMIT -DHAVE_STRNLEN  -I/usr/include -DHAVE_MONOTONIC_CLOCK -c $(OUTPUT_OPTION) $<

# missing common object files
$(COMMON_DIR_OBJ):
	@$(MAKE) -C ../common server

$(LIBCONFIG_AR):
	@$(MAKE) -C ../../3rdparty/libconfig

$(YAML_CPP_AR):
	@$(MAKE) -C ../../3rdparty/yaml-cpp
