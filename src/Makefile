CC = gcc
TESTFLAGS = -lcheck -g
CFLAGS = -Wall -Wextra -Werror -std=c11
TESTFLAGS2 = -lcheck -lm -lpthread -lrt -lsubunit -g
GCOVFLAGS = -fprofile-arcs -ftest-coverage
PROGNAME = front

all: test uninstall install

s21_3dviewer.a : func/*.c
	$(CC) $(CFLAGS) -g -std=c11 -c func/*.c
	ar rcs s21_3dviewer.a *.o
	rm -rf *.o

test: clean tests/test.c func/*.c
	$(CC) $(CFLAGS) -lcheck tests/test.c func/*.c -g -o test $(TESTFLAGS)
	./test

gcov_report:
	$(CC) $(GCOVFLAGS) $(CFLAGS) tests/test.c func/*.c -o test $(TESTFLAGS)
	./test
	lcov -t "test" -o test.info -c -d .
	genhtml -o report test.info
	rm -rf gcovreport gcovreport.info *.gcda *.gcno string_test.gcda string_test.gcno
	open report/index.html

rebuild: clean all

style_check:
	find . -name "*.c" -o -name "*.h" -o -name "*.cpp" | xargs clang-format -style=Google -n

style_set:
	find . -name "*.c" -o -name "*.h" -o -name "*.cpp" | xargs clang-format -style=Google -i

install: uninstall
	cd front/ && qmake && make && make clean
	mv front/$(PROGNAME).app build

open:
	./build/Contents/MacOS/$(PROGNAME)

uninstall: clean
	cd front && rm -rf Makefile $(PROGNAME).app $(PROGNAME).pro.user .qmake.stash .qtc_clangd .tmp .qm
	rm -rf build

dist: install
	rm -rf Archive/
	mkdir Archive/
	mv build/$(PROGNAME).app Archive/
	tar -cvzf Archive.tar.gz Archive

dvi:
	open docs/index.html

valgrind:
	valgrind --vgdb=no --leak-check=full --show-reachable=yes --show-leak-kinds=all --track-origins=yes --verbose --read-var-info=yes ./build/$(PROGNAME).app/Contents/MacOS/$(PROGNAME)

leaks:
	leaks -atExit -- ./build/Contents/MacOS/front

clean: 
	rm -rf *.o front/calc.pro.*
	rm -rf front/*.o
	rm -rf front/moc_*
	rm -rf *.o build*
	rm -rf *.o test
	rm -rf *.o *.a
	rm -rf *.o a.out
	rm -rf *.o *.gcno
	rm -rf *.o *.gcda
	rm -rf *.o test.dSYM
	rm -rf *.o *.info
	rm -rf *.o report
	rm -rf Archive
	rm -rf Archive.tar.gz
	clear

main: clean
	$(CC) $(CFLAGS) -c func/*.c
	$(CC) 
	$(CFLAGS) main.c *.o -o main
	./main