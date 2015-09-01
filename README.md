# How to use it

## Requirements

1. Node : v0.10.35 or later

**Install nodejs by nvm**

install nvm script using cURL:

	curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.26.1/install.sh | bash
or Wget:

	wget -qO- https://raw.githubusercontent.com/creationix/nvm/v0.26.1/install.sh | bash

install nodejs and use it:

	nvm install stable

2. Hexo CLI : v0.1.4 or later

	npm install hexo-cli -g

## Install and deploy blog

install dependencies:

	cd <folder>
	npm install
	npm install hexo-generator-feed --save
	npm install hexo-deployer-git --save

view on the local host:

	hexo s

deploy it to github pages:

	hexo g -d


**Enjoy it!**