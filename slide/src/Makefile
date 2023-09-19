.PHONY: live build clean

dark =
ifdef dark
	file_md = main.dark.md
else
	file_md = main.md
endif


live:
	@echo "Previewing main slides..."
	-@reveal-md $(file_md) -w --scripts https://cdn.jujimeizuo.cn/heti/heti.js,heti_worker.js --template template.html || true

build:
	@echo "Building main slides..."
	@reveal-md $(file_md) --scripts https://cdn.jujimeizuo.cn/heti/heti.js,heti_worker.js --template template.html --static ../site --assets-dir assets
	@rm ../site/main.html

clean:
	@echo "Cleaning up..."
	rm -rf ../site/
