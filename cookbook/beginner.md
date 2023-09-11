```makefile
# Makefile Cookbook

# 1. Basic Variables
CC := gcc
CFLAGS := -Wall -O2

# 2. Targets and Dependencies
my_program: main.o util.o
	$(CC) $^ -o $@			

main.o: main.c
	$(CC) $(CFLAGS) -c $< -o $@

util.o: util.c
	$(CC) $(CFLAGS) -c $< -o $@

# 3. Pattern Rules
%.o: %.c
	$(CC) $(CFLAGS) -c $< -o $@		

# 4. Phony Targets
.PHONY: clean test

clean:
	rm -f *.o my_program

test:
	./run_tests.sh

# 5. Conditional Build
ifdef DEBUG
CFLAGS += -g
endif

# 6. Automatic Variables
SRCS := main.c util.c
OBJS := $(SRCS:.c=.o)

# 7. Wildcard Function
SOURCE_FILES := $(wildcard *.c)

# 8. VPATH for Source Files
VPATH := src:lib

# 9. Command Echoing
.SILENT:

# 10. .DELETE_ON_ERROR
.DELETE_ON_ERROR:

# 11. User-Specified Variables
ifeq ($(OS),Windows_NT)
	CC := x86_64-w64-mingw32-gcc
endif

# 12. Conditional Inclusion of Other Makefiles
ifdef USE_ADDITIONAL_MAKEFILE
	include additional.mk
endif

# 13. Special Targets
.DEFAULT_GOAL := my_program
```