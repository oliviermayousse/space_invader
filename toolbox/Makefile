# ==========================================
#   Unity Project - A Test Framework for C
#   Copyright (c) 2007 Mike Karlesky, Mark VanderVoord, Greg Williams
#   [Released under MIT License. Please refer to license.txt for details]
# ==========================================

#We try to detect the OS we are running on, and adjust commands as needed
ifeq ($(OS),Windows_NT)
  ifeq ($(shell uname -s),) # not in a bash-like shell
	CLEANUP = del /F /Q
	MKDIR = mkdir
  else # in a bash-like shell, like msys
	CLEANUP = rm -rf
	MKDIR = mkdir -p
  endif
	TARGET_EXTENSION=.dll	
	TARGET_TEST_EXTENSION=.exe
else
	CLEANUP = rm -rf
	MKDIR = mkdir -p
	TARGET_EXTENSION=.so
	TARGET_TEST_EXTENSION=.out
endif

C_COMPILER=gcc
ifeq ($(shell uname -s), Darwin)
C_COMPILER=clang
endif

UNITY_ROOT=test/unity.framework/Unity
UNIT_TESTS_ROOT=test/unit_tests

CFLAGS=-std=c99
CFLAGS += -g
CFLAGS += -Wall
CFLAGS += -Wextra
CFLAGS += -Wpointer-arith
CFLAGS += -Wcast-align
CFLAGS += -Wwrite-strings
CFLAGS += -Wswitch-default
CFLAGS += -Wunreachable-code
CFLAGS += -Winit-self
CFLAGS += -Wmissing-field-initializers
CFLAGS += -Wno-unknown-pragmas
CFLAGS += -Wstrict-prototypes
CFLAGS += -Wundef
CFLAGS += -Wold-style-definition
#CFLAGS += -Wno-misleading-indentation

CFLAG_COVERAGE=--coverage

CFLAGS_LIB= -shared
CFLAGS_LIB+= -O2
CFLAGS_LIB+= -fPIC

TARGET_LIB_BASE= libgametoolbox
TARGET_LIB= $(TARGET_LIB_BASE)$(TARGET_EXTENSION)

TARGET_TEST_BASE= AllTests
TARGET_TEST= $(TARGET_TEST_BASE)$(TARGET_TEST_EXTENSION)

TARGET_COV_DIR= .
#TARGET_COV_DIR= coverage
TARGET_COV= $(TARGET_TEST_BASE)Cov$(TARGET_TEST_EXTENSION)

SRC_LIB=src/Settings.c \
  	src/Game.c \
	src/Menu.c

SRC_TEST_FILES=\
  $(UNITY_ROOT)/src/unity.c \
  $(UNITY_ROOT)/extras/fixture/src/unity_fixture.c \
  $(UNIT_TESTS_ROOT)/TestSettings.c \
  $(UNIT_TESTS_ROOT)/TestGame.c \
  $(UNIT_TESTS_ROOT)/test_runners/TestSettings_Runner.c \
  $(UNIT_TESTS_ROOT)/test_runners/TestGame_Runner.c \
  $(UNIT_TESTS_ROOT)/test_runners/AllTests.c

INC_TEST_DIRS=-Isrc -I$(UNITY_ROOT)/src -I$(UNITY_ROOT)/extras/fixture/src
INC_COV_DIRS=-I../src -I$(UNITY_ROOT)/src -I$(UNITY_ROOT)/extras/fixture/src
SYMBOLS=-DUNITY_FIXTURE_NO_EXTRAS

all: clean test lib

lib:
	$(C_COMPILER) $(CFLAGS) $(CFLAGS_LIB) $(INC_DIRS) $(SYMBOLS) $(SRC_LIB) -o $(TARGET_LIB)

tests:
	$(C_COMPILER) $(CFLAGS) $(INC_TEST_DIRS) $(SYMBOLS) $(SRC_LIB) $(SRC_TEST_FILES) -o $(TARGET_TEST)
	- ./$(TARGET_TEST) -v

coverages:
	$(MKDIR) $(TARGET_COV_DIR)
	cd $(TARGET_COV_DIR)
	$(C_COMPILER) $(CFLAGS) $(CFLAG_COVERAGE) $(INC_COV_DIRS) $(SYMBOLS) $(SRC_LIB) $(SRC_TEST_FILES) -o $(TARGET_COV)
	- ./$(TARGET_COV) -v

clean: cleanlib cleantests cleancov
cleanlib:
	$(CLEANUP) $(TARGET_LIB)
cleantests:
	$(CLEANUP) $(TARGET_TEST)
cleancov:
	$(CLEANUP) $(TARGET_COV_DIR)/*.gc*
	$(CLEANUP) $(TARGET_COV)


