# Sun 18 Oct 2020 09:09:15 PM PDT

installing jekyll and other reqs for working on jekyll website locally
I am referencing this website: https://www.digitalocean.com/community/tutorials/how-to-set-up-a-jekyll-development-site-on-ubuntu-20-04

```bash
sudo apt -y install make build-essential ruby ruby-dev

nano .bashrc

# add this to .bachrc
export GEM_HOME=$HOME/gems
export PATH=$HOME/gems/bin:$PATH

source ~/.bashrc
gem install jekyll bundler
```

some important things to note for the future. I should not install ruby/jekyll stuff using conda. If for whatever reason it starts throwing errors, the easiest thing is probably to remove ruby/jekyll and then delete the hidden folders/file such as gems and .gems then reinstall per the jekyll website.

On another note: removing anaconda is also a pain and I should be sure to delete the hidden files after removing those too. 


# Fri 20 Nov 2020 12:28:49 AM PST

as a reminder for how to run a local instance of the website
```bash
bundle exec jekyll serve
```