# Copyright 2020 Alex Woroschilow (alex.woroschilow@gmail.com)
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
# http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
PWD:=$(shell pwd)

.PHONY: all

all: clean init

	mkdir --parents $(PWD)/build/Boilerplate.AppDir	
	$(PWD)/bin/Apprepo.AppImage --destination=$(PWD)/build appdir boilerplate \
	    libwxgtk3.0-gtk3-0v5 libxi6 libpcre3 libgtk-3-0 libicu-dev \
		libselinux1 libwayland-cursor0

	echo ''                                  >> $(PWD)/build/Boilerplate.AppDir/AppRun
	echo ''                                  >> $(PWD)/build/Boilerplate.AppDir/AppRun
	echo 'exec $${APPDIR}/bin/far2l "$${@}"' >> $(PWD)/build/Boilerplate.AppDir/AppRun

	mkdir --parents $(PWD)/build/far2l
	git clone https://github.com/elfmz/far2l $(PWD)/build/far2l

	mkdir --parents $(PWD)/build/far2l/build
	mkdir --parents $(PWD)/build/far2l/dist

	cd $(PWD)/build/far2l/build && cmake -DCMAKE_INSTALL_PREFIX=$(PWD)/build/far2l/dist -DUSEWX=yes -DCMAKE_BUILD_TYPE=Release ..
	cd $(PWD)/build/far2l/build && cmake --build . -j2
	cd $(PWD)/build/far2l/build && cmake --install .

	cp --force --recursive $(PWD)/build/far2l/dist/bin/*   $(PWD)/build/Boilerplate.AppDir/bin
	cp --force --recursive $(PWD)/build/far2l/dist/lib/*   $(PWD)/build/Boilerplate.AppDir/lib64
	cp --force --recursive $(PWD)/build/far2l/dist/share/* $(PWD)/build/Boilerplate.AppDir/share

	rm --force $(PWD)/build/Boilerplate.AppDir/*.svg                  || true
	rm --force $(PWD)/build/Boilerplate.AppDir/*.desktop              || true
	rm --force $(PWD)/build/Boilerplate.AppDir/*.png                  || true

	cp --force $(PWD)/AppDir/*.png 		$(PWD)/build/Boilerplate.AppDir/ || true
	cp --force $(PWD)/AppDir/*.desktop 	$(PWD)/build/Boilerplate.AppDir/ || true
	cp --force $(PWD)/AppDir/*.svg 		$(PWD)/build/Boilerplate.AppDir/ || true

	export ARCH=x86_64 && $(PWD)/bin/appimagetool.AppImage $(PWD)/build/Boilerplate.AppDir $(PWD)/Far2l.AppImage
	chmod +x $(PWD)/Far2l.AppImage

init:
	mkdir --parents $(PWD)/bin

	wget https://github.com/area-of-dev/apprepo.AppImage/releases/download/latest/apprepo.AppImage \
		--output-document=$(PWD)/bin/Apprepo.AppImage
	chmod +x $(PWD)/bin/Apprepo.AppImage

	wget https://github.com/AppImage/AppImageKit/releases/download/13/appimagetool-x86_64.AppImage \
		--output-document=$(PWD)/bin/appimagetool.AppImage
	chmod +x $(PWD)/bin/appimagetool.AppImage

clean:
	rm -rf $(PWD)/*.AppImage
	rm -rf $(PWD)/bin/*
	rm -rf $(PWD)/build
