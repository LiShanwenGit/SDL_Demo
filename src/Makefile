###### Makefile ######

######################################
# target
######################################
TARGET =sdl_demo

######################################
# building variables
######################################
# debug build?
DEBUG = 0
# optimization
OPT = -O2#-flto

# Build path
SRC    := main
SDL    := ../sdl


######################################
# source
######################################
# C sources
C_SOURCES =  \
$(wildcard $(SRC)/*.c)  \


# C defines
C_DEFS = \
-D
# AS includes
AS_INCLUDES = 

# C includes
C_INCLUDES =     \
-I.              \
-I$(SRC)          \
-I$(SDL)/include/SDL2  \


BUILD_DIR    := obj
#######################################
# Compiler Setting
#######################################
PREFIX =
CC = $(PREFIX)g++
AS = $(PREFIX)g++ -x assembler-with-cpp
CP = $(PREFIX)objcopy
AR = $(PREFIX)ar
SZ = $(PREFIX)size
OD = $(PREFIX)objdump
HEX =$(CP) -O ihex
BIN =$(CP) -O binary -S

LIB_DIR = \
-L$(SDL)/lib  \
-L$(SDL)/bin  \

LIBS    = \
$(SDL)/lib/libSDL2.a        \
$(SDL)/lib/libSDL2.dll.a    \
$(SDL)/lib/libSDL2_test.a   \
$(SDL)/lib/libSDL2main.a    \


LIB_OPT = -lmingw32 -lSDL2main -lSDL2.dll -luser32 -lgdi32 -lwinmm -ldxguid  

CFLAGS := -Wall  $(C_INCLUDES)  

ifeq ($(DEBUG), 1)
CFLAGS += -g -gdwarf-2 
endif

# libraries
#LIBS = -lc_nano -lm
LDFLAGS = $(OPT)   $(LIB_DIR) $(LIB_OPT) $(LIBS)

# default action: build all
all: $(TARGET).exe

#######################################
# build the application
#######################################
# list of objects
OBJECTS = $(addprefix $(BUILD_DIR)/,$(notdir $(C_SOURCES:.c=.o)))
vpath %.c $(sort $(dir $(C_SOURCES)))


$(BUILD_DIR)/%.o: %.c Makefile | $(BUILD_DIR)
	@echo "CC $<"
	@$(CC) -c $(CFLAGS) -MMD -MP \
		-MF  $(BUILD_DIR)/$(notdir $(<:.c=.d)) \
		-Wa,-a,-ad,-alms=$(BUILD_DIR)/$(notdir $(<:.c=.lst)) $< -o $@

$(TARGET).exe: $(OBJECTS) Makefile
	@echo "LD $@"
	@$(CC) $(OBJECTS) $(LDFLAGS) -o $@
	@echo "SIZE $@"
	@$(SZ) $@
	@cp $(SDL)/bin/SDL2.dll ./

$(BUILD_DIR):
	mkdir $@

#######################################
# clean up
#######################################

clean:
	-rm -fR $(BUILD_DIR)