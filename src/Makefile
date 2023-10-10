.PHONY: all clean

CC=gcc
FLAGS=-Wall -Wextra -Werror
LDFLAGS=$(shell pkg-config --cflags --libs check)

GCOVFLAGS=
LIBNAME=s21_string
SRCS=$(wildcard *.c)
GCOVNAME=coverage

all: s21_string.a

$(LIBNAME).a:
	$(CC) $(FLAGS) $(GCOVFLAGS) -c $(SRCS)
	ar rc s21_string.a *.o
	ranlib s21_string.a
	rm *.o
	
clean:
	-rm -rf gcov_report/ *.o *.html *.gcda *.gcno *.css *.a *.gcov *.info *.out *.cfg *.txt gcov_main ./test

debug:
	$(CC) -g $(FLAGS) $(SRCS)make clean

style:
	clang-format -style=Google -n *.c *.h tests/*.c *.h 

style_check:
	clang-format -style=Google -n *.c *.h tests/*.c *.h 


test:
	$(CC) $(FLAGS) $(LDFLAGS) $(SRCS) tests/*.c -o test
	./test

gcov_report: add_coverage_flag
	$(CC) $(FLAGS) $(LDFLAGS) $(GCOVFLAGS) $(SRCS) tests/*.c -o gcov_main
	./gcov_main
	lcov --capture --directory . --output-file coverage.info
	genhtml coverage.info --output-directory gcov_report
	open gcov_report/index.html
	

test2: $(LIBNAME).a
	$(CC) $(FLAGS) $(LDFLAGS) -L. $(LIBNAME).a tests/*.c -o test2
	./test2

add_coverage_flag:
	$(eval FLAGS += --coverage)

gcov_report2: add_coverage_flag test2
	gcov -b -l -p -c s21_*.gcno
	lcov -o $(GCOVNAME).info -c -d .
	genhtml -o $(GCOVNAME) $(GCOVNAME).info
	open $(GCOVNAME)/index.html
	rm -rf *.gcda *.gcno *.gcov *.info
