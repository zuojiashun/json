#
# NOTE: This Makefile is just an example, intended for the regression
# utility, and is not likely to work in your environment because there is no
# standard location for the libffi headers and library.
#
# The ffi.h in OS X is broken and needs to be patched:
#
# --- /usr/include/ffi/ffi.h.xcode	2011-05-25 09:12:43.000000000 -0700
# +++ /usr/include/ffi/ffi.h	2012-03-10 20:18:54.000000000 -0800
# @@ -302,7 +302,7 @@
#  	void				(*fun)(ffi_cif*,void*,ffi_raw*,void*),
#  	void*				user_data);
#  
# -#elif
# +#else
#  #error FFI_CLOSURES not defined
#  #endif
#

cc-option = $(shell if $(CC) $(1) -S -o /dev/null -xc /dev/null \
              > /dev/null 2>&1; then echo "$(1)"; else echo "$(2)"; fi;)

override CPPFLAGS += -D_GNU_SOURCE -DJSON_MAIN

ifeq (SunOS,$(shell uname -s))
override CFLAGS +=
else
override DFLAGS += -Wall -Wextra -O3 -g
override CFLAGS += $(call cc-option, -std=c11, -std=gnu99) $(DFLAGS)
endif

override LDFLAGS += 
override LDLIBS  += -lffi

ifeq (Linux,$(shell uname -s))
override CPPFLAGS += -DFFI_H_PATH=\<ffi.h\>
override LDLIBS   += -lm
endif

ifeq (OpenBSD,$(shell uname -s))
override CPPFLAGS += -I/usr/local/include -DFFI_H_PATH=\<ffi.h\>
override LDFLAGS  += -L/usr/local/lib
override LDLIBS   += -pthread
endif

ifeq (SunOS,$(shell uname -s))
override LDLIBS += -lm
endif


all: json

json: json.c json.h
	$(CC) $(CFLAGS) -o $@ $< $(CPPFLAGS) $(LDFLAGS) $(LDLIBS)


.PHONY: clean clean~

clean:
	rm -rf json *.o *.dSYM

clean~: clean
	rm -f *~
