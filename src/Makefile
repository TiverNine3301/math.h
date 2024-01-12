PROJECT_NAME = s21_math
CC = gcc
OS = $(shell uname)

ifeq ($(OS), Linux)
	FLAGS = -Werror -Wall -Wextra -std=c11 -lm -lsubunit
	TEST_FLAGS = -lcheck -pthread -lrt -lm -lsubunit
else
	FLAGS = -Werror -Wall -Wextra -std=c11
	TEST_FLAGS = -lcheck
endif

.PHONY: all clean test s21_string.a gcov_report

all: $(PROJECT_NAME).a

$(PROJECT_NAME).a:
	$(CC) $(FLAGS) -c s21_math.c
	ar rcs $(PROJECT_NAME).a s21_math.o

test:$(PROJECT_NAME).a
	$(CC) $(FLAGS) test_s21math.c $(PROJECT_NAME).a -o s21_test $(TEST_FLAGS)
	./s21_test

check: test
	cppcheck --enable=all --suppress=missingIncludeSystem --inconclusive --check-config *.c *.h
	make test
	leaks --atExit -- test

gcov_report: clean
	$(CC) $(FLAGS) -c test_s21math.c
	$(CC) $(FLAGS) --coverage test_s21math.o s21_math.c -o s21_test $(TEST_FLAGS)
	./s21_test
	lcov -d ./ --capture --output-file $(PROJECT_NAME).info
	genhtml $(PROJECT_NAME).info --output-directory report
	open report/index.html

style: clean
	cp ../materials/linters/.clang-format ./
	clang-format -n *.c
	clang-format -n *.h
	rm -rf .clang-format

clang: clean
	cp ../materials/linters/.clang-format ./
	clang-format -i *.c
	clang-format -i *.h
	rm -rf .clang-format

clean:
	rm -rf *.a *.out *.o s21_test *.html *.css
	rm -rf *.g* *.info report
