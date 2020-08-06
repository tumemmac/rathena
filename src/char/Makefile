
COMMON_H = $(shell ls ../common/*.hpp)
COMMON_AR = ../common/obj/common.a
COMMON_INCLUDE = -I../common/

LIBCONFIG_H = $(shell ls ../../3rdparty/libconfig/*.h)
LIBCONFIG_AR = ../../3rdparty/libconfig/obj/libconfig.a
LIBCONFIG_INCLUDE = -I../../3rdparty/libconfig

COMMON_OBJ = ../common/obj/sql.o
COMMON_H = ../common/sql.hpp

CHAR_OBJ = $(shell ls *.cpp | sed -e "s/\.cpp/\.o/g")
CHAR_DIR_OBJ = $(CHAR_OBJ:%=obj/%)
CHAR_H = $(shell ls ../char/*.hpp)

YAML_CPP_OBJ = $(shell find ../../3rdparty/yaml-cpp/ -type f -name "*.cpp" | sed -e "s/\.cpp/\.o/g" )
YAML_CPP_DIR_OBJ = $(YAML_CPP_OBJ:%=obj/%)
YAML_CPP_AR = ../../3rdparty/yaml-cpp/obj/yaml-cpp.a
YAML_CPP_H = $(shell find ../../3rdparty/yaml-cpp/ -type f -name "*.h")
YAML_CPP_INCLUDE = -I../../3rdparty/yaml-cpp/include

HAVE_MYSQL=yes
ifeq ($(HAVE_MYSQL),yes)	
	SERVER_DEPENDS=char-server
else
	SERVER_DEPENDS=needs_mysql
endif
ALL_DEPENDS=server



#####################################################################
.PHONY : all server clean help

all: $(ALL_DEPENDS)

server: $(SERVER_DEPENDS)

clean:
	@echo "	CLEAN	char"
	@rm -rf *.o obj ../../char-server

help:
	@echo "possible targets are 'server' 'all' 'clean' 'help'"
	@echo "'server'    - char server"
	@echo "'all'       - builds all above targets"
	@echo "'clean'     - cleans builds and objects"
	@echo "'help'      - outputs this message"

#####################################################################


char-server: obj $(CHAR_DIR_OBJ) $(LIBCONFIG_AR) $(COMMON_AR) $(YAML_CPP_AR)
	@echo "	LD	char-server"
	@g++  -L/usr/lib -o ../../char-server $(CHAR_DIR_OBJ) $(LIBCONFIG_AR) $(COMMON_AR) $(LIBCONFIG_OBJ) $(YAML_CPP_AR) -lpcre -lpthread -lz  -ldl -L/usr/lib64/mysql -lmysqlclient -lpthread -lz -lm -ldl -lssl -lcrypto

needs_mysql:
	@echo "MySQL not found or disabled by the configure script"
	@exit 1

obj:
	@echo "	MKDIR	obj"
	@-mkdir obj
	
obj/%.o: %.cpp $(CHAR_H) $(COMMON_H) $(LIBCONFIG_H) $(YAML_CPP_H)
	@echo "	CXX	$<"
	@g++ -g -O2 -std=c++11 $(COMMON_INCLUDE) $(LIBCONFIG_INCLUDE) $(YAML_CPP_INCLUDE) -I/usr/include/mysql  -I../common -DHAS_TLS -fno-strict-aliasing -DHAVE_SETRLIMIT -DHAVE_STRNLEN  -I/usr/include -DHAVE_MONOTONIC_CLOCK -c $(OUTPUT_OPTION) $<

# missing object files
$(COMMON_AR):
	@$(MAKE) -C ../common server

$(LIBCONFIG_AR):
	@$(MAKE) -C ../../3rdparty/libconfig
	
$(YAML_CPP_AR):
	@$(MAKE) -C ../../3rdparty/yaml-cpp
