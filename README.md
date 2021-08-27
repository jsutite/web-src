# JSUTITE Homepage

Source code for [jsutite.github.io](https://jsutite.github.io/).

```
git clone https://github.com/jsutite/web-src.git
mkdir docs
cd web-src
bundle install
bundle exec jekyll build -d ../docs
```

## 注意

环境采用Ubuntu 20.04 + Ruby 2.7

```
sudo apt-get install ruby-full build-essential ruby-bundler
```
生成的页面文件在`docs`中，按需求`git push`。