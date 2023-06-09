-include ../../Makefile.include
include UDFREAD-VERSION
DEPS = Makefile UDFREAD-VERSION ../../download-files.include ../../download-files.include

ifeq ($(CROSS_COMPILING), yes)
  DEPS += ../../Makefile.include
else
  ROOT_DIR := $(shell dirname $(realpath $(lastword $(MAKEFILE_LIST))))
  TARBALLS_LOCATION ?= $(ROOT_DIR)
  BASE_URL ?= http://mirrors.kodi.tv/build-deps/sources
  RETRIEVE_TOOL := curl -Ls --retry 10 --retry-delay 3 -o $(ARCHIVE)
  ARCHIVE_TOOL := tar --strip-components=1 -xf
endif

include ../../download-files.include

# configuration settings
CONFIGURE = ./configure --prefix=$(PREFIX) --disable-shared --enable-static

LIBDYLIB=$(PLATFORM)/.libs/$(BYPRODUCT)

all: .installed-$(PLATFORM)
download: $(TARBALLS_LOCATION)/$(ARCHIVE)

$(PLATFORM): $(DEPS) | $(TARBALLS_LOCATION)/$(ARCHIVE).$(HASH_TYPE)
	rm -rf $(PLATFORM)/*; mkdir -p $(PLATFORM)
	cd $(PLATFORM); $(ARCHIVE_TOOL) $(ARCHIVE_TOOL_FLAGS) $(TARBALLS_LOCATION)/$(ARCHIVE)
	cd $(PLATFORM); $(AUTORECONF) -vif
	cd $(PLATFORM); $(CONFIGURE)

$(LIBDYLIB): $(PLATFORM)
	$(MAKE) -C $(PLATFORM)

.installed-$(PLATFORM): $(LIBDYLIB)
	$(MAKE) -C $(PLATFORM) install
	touch $@

clean:
	$(MAKE) -C $(PLATFORM) clean
	rm -f .installed-$(PLATFORM)

distclean:
	rm -rf $(PLATFORM) .installed-$(PLATFORM)
