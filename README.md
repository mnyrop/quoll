# quoll
capybara tests for lunrjs indexes 🐀🌒

![eastern quoll](https://upload.wikimedia.org/wikipedia/en/3/39/Eastern_Quoll_%28Fawn%29.JPG)

### purpose:

headlessly test your client-side **[lunr](https://lunrjs.com/)**/**[elasticlunr](http://elasticlunr.com/)** search index on your **[jekyll](jekyllrb.com)** site using **[rspec](http://rspec.info/)**, **[capybara](http://teamcapybara.github.io/capybara/)**, and **[selenium webdriver](https://www.seleniumhq.org/projects/webdriver/)**.

### quick start:

`Gemfile`:

```
source 'https://rubygems.org'
gem 'jekyll'

group :development, :test do
  gem 'rspec'
  gem 'selenium-webdriver'
  gem 'chromedriver-helper'
  gem 'capybara'
  gem 'rack-jekyll'
end
```

`_config.yml`:

```yaml
quoll:
  main:
    page: '/' # the path of the page you want to test
    terms:  # the search terms you expect to yield results
      - 'capybara'
      - 'testing'
      - 'example'
```
