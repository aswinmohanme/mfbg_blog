A Mother Fucking Blog Generator
===============================
2018-11-20

This is a mother fucking blog post and it's been generated from markdown by my
fucking blog generator.

### It can
- Generate your html pages from markdown because fuck writing blogs in raw html
- Generate an index.html containing a list of all the posts you've written

That's it. What more do you need, it's a fucking blog.

### How to use it
1. Fork [jeremycw/mfbg](https://github.com/jeremycw/mfbg)
2. Install pandoc: `brew install pandoc` because fuck you, I wanted markdown.
3. Add some posts, change the layout, do whatever the fuck you want.
4. run `make`

Here it is, all 37 glorious lines of make:

```
POST_DIR=posts
LAYOUT_DIR=layout
OUTPUT_DIR=site
BUILD_DIR=build

POSTS=$(notdir $(wildcard $(POST_DIR)/*.md))
POSTS_HTML=$(POSTS:%.md=$(OUTPUT_DIR)/%.html)

.PHONY: clean

all: $(POSTS_HTML) $(OUTPUT_DIR)/index.html

$(OUTPUT_DIR)/index.html: $(LAYOUT_DIR)/index_header.html $(LAYOUT_DIR)/index_footer.html $(POST_DIR)
	cp $(LAYOUT_DIR)/index_header.html $@
	for filename in `ls -t $(POST_DIR)`; do \
		f=$(POST_DIR)/$$filename; \
		echo "<a href=\"/$$(basename $$f .md)\">`sed '3q;d' $$f` - `sed '1q;d' $$f`</a><br/>" >> $@; \
	done
	cat $(LAYOUT_DIR)/index_footer.html >> $@

$(OUTPUT_DIR)/%.html: $(BUILD_DIR)/%.html $(BUILD_DIR)/%_header.html $(LAYOUT_DIR)/footer.html | $(OUTPUT_DIR)
	cat $(BUILD_DIR)/$*_header.html $< $(LAYOUT_DIR)/footer.html > $@

$(BUILD_DIR)/%_header.html: $(POST_DIR)/%.md $(LAYOUT_DIR)/header.html
	sed "s/{title}/$(shell head -1 $<)/g" $(LAYOUT_DIR)/header.html > $@

$(BUILD_DIR)/%.html: $(POST_DIR)/%.md | $(BUILD_DIR)
	pandoc -f markdown+pipe_tables -t html --ascii $< > $@

$(OUTPUT_DIR):
	mkdir -p $@

$(BUILD_DIR):
	mkdir -p $@

clean:
	@rm -rf $(OUTPUT_DIR) $(BUILD_DIR)
```

### Stupid Questions
- **Q:** But where are my rubies and my gems and my npms and webpacks and transpilers?
- **A:** Fuck you, you don't need any of that shit. This uses make cause that shit's fucking everywhere
- **Q:** Where are the .yml and .json files to configure it?
- **A:** It's 37 fucking lines, just change the fucking Makefile
- **Q:** Does it work on linux?
- **A:** I don't know, probably. Just fucking try it. If it doesn't fucking fix it.
