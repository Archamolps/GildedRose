# Makefile for building the kata file with the Google Testing Framework
#
# SYNOPSIS:
#
#   make [all]  - makes everything, runs tests
#   make TARGET - makes the given target.
#   make clean  - removes all files generated by make.
#   make memtest - run memory leak analysis

# The _POSIX_C_SOURCE definition prevents the compiler from throwing warnings
CFLAGS = `pkg-config --cflags check` -g --std=c99 -D_POSIX_C_SOURCE=200809L
LIBS = `pkg-config --libs check`

# All files that should be part of your test should start with 'test_'
TEST_SRC = $(wildcard test_*.c)
TEST_BASE = $(basename ${TEST_SRC})
TEST_OBJECTS = $(addsuffix .o, ${TEST_BASE})

# All files that should be part of your main program should start with 'gilded_'
PROG_SRC = $(wildcard gilded_*.c)
PROG_BASE = $(basename ${PROG_SRC})
PROG_OBJECTS = $(addsuffix .o, ${PROG_BASE})

OBJECT_UNDER_TEST = GildedRose.o ${PROG_OBJECTS}

# This is the test application.  You can run this program to see your test output
TEST_APP = test_gildedrose


# This will generate output for several products over a course of several days.
# You can run this application to build golden rule tests
GOLDEN_APP = golden_rose

all: ${TEST_APP} ${GOLDEN_APP}
	./${TEST_APP}

${TEST_APP}: ${TEST_OBJECTS} ${OBJECT_UNDER_TEST}
	$(CC) $(CFLAGS) -o $@ $^ $(LIBS)

${GOLDEN_APP}: GildedRoseTextTests.o ${OBJECT_UNDER_TEST}
	$(CC) $(CFLAGS) -o $@ $^

# If you're not on a mac, you should run memtest (in fact, consider adding it to the 'all' target).
# If you're on a mac, complain to apple for breaking an important development tool.
memtest: ${TEST_APP}
	valgrind --leak-check=full --error-exitcode=1 ./${TEST_APP} --nofork

clean:
	rm -f *.o
	rm -f ${TEST_APP}
	rm -f ${GOLDEN_APP}
