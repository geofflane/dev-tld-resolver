CC=gcc
CFLAGS=-O2

OBJ=nss_test_tld.o

%.o: %.c
	$(CC) -Wall -fPIC -c -o $@ $< $(CFLAGS)

libnss_test_tld.so.2: $(OBJ)
	$(CC) -shared -o $@ $^ -Wl,-soname,libnss_test_tld.so.2 $(CFLAGS)

clean:
	rm -f *.o *~ libnss_test_tld.so.2

install: libnss_test_tld.so.2
	ln -sf $(CURDIR)/libnss_test_tld.so.2 /lib

uninstall:
	rm /lib/libnss_test_tld.so.2

