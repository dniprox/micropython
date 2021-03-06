include ../../py/mkenv.mk
-include mpconfigport.mk

# define main target
PROG = micropython.exe

# qstr definitions (must come before including py.mk)
QSTR_DEFS = ../unix/qstrdefsport.h

# include py core make definitions
include $(TOP)/py/py.mk

INC += -I.
INC += -I$(TOP)
INC += -I$(BUILD)

# compiler settings
CFLAGS = $(INC) -Wall -Wpointer-arith -Werror -std=gnu99 -DUNIX -D__USE_MINGW_ANSI_STDIO=1 $(CFLAGS_MOD) $(COPT)
LDFLAGS = $(LDFLAGS_MOD) -lm

# Debugging/Optimization
ifdef DEBUG
CFLAGS += -g
COPT = -O0
else
COPT = -Os #-DNDEBUG
endif

# source files
SRC_C = \
	lib/utils/printf.c \
	ports/unix/main.c \
	ports/unix/file.c \
	ports/unix/input.c \
	ports/unix/modos.c \
	ports/unix/modmachine.c \
	ports/unix/modtime.c \
	ports/unix/gccollect.c \
	windows_mphal.c \
	realpath.c \
	init.c \
	sleep.c \

OBJ = $(PY_O) $(addprefix $(BUILD)/, $(SRC_C:.c=.o))

ifeq ($(MICROPY_USE_READLINE),1)
CFLAGS_MOD += -DMICROPY_USE_READLINE=1
SRC_C += lib/mp-readline/readline.c
endif

LIB += -lws2_32

# List of sources for qstr extraction
SRC_QSTR += $(SRC_C)
# Append any auto-generated sources that are needed by sources listed in
# SRC_QSTR
SRC_QSTR_AUTO_DEPS +=

include $(TOP)/py/mkrules.mk
