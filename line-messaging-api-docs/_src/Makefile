
MD_FILES = $(shell find *.md)
HTML_FILES = $(MD_FILES:%.md=../%.html)
IMG_DIR = ../_img

all: $(HTML_FILES) $(IMG_DIR)

clean:
	rm -f $(HTML_FILES)

../%.html: %.md
	grip --export $< $@

../_img: _img
	cp -rf _img ../