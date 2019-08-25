# Snowkylin's Blog

```sh
# install ruby in Ubuntu https://jekyllrb.com/docs/installation/ubuntu/
sudo apt-get update
sudo apt-get install ruby-full build-essential zlib1g-dev
echo '# Install Ruby Gems to ~/gems' >> ~/.bashrc
echo 'export GEM_HOME="$HOME/gems"' >> ~/.bashrc
echo 'export PATH="$HOME/gems/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
# install jekyll and bundler with gem
gem install jekyll bundler
# set mirror in China
bundle config mirror.https://rubygems.org https://gems.ruby-china.com
# cd to blog directory
cd /mnt/c/Users/xihan/OneDrive/snowkylin.github.io
# install dependencies in Gemfile
bundle install
# run jekyll
bundle exec jekyll serve
```