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
  gem 'geckodriver-helper'
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

sample `_includes/search.html`:

```html
<script src="{{ site.js.elasticlunr.cdn | default: 'https://cdnjs.cloudflare.com/ajax/libs/elasticlunr/0.9.6/elasticlunr.min.js' }}"></script>
<script src="{{ site.baseurl }}/js/lunr-ui.js"></script>

<div class="search-block">
  <div class="input-group">
    <input type="text" class="form-control" id="search" name="x" placeholder="Search... ">
  </div>
  <div id="results"></div>
</div>
```

sample `js/lunr-ui.js`:

```js
---
layout: none
---
$.getJSON("{{ site.baseurl }}/js/lunr-index.json", function(index_json) {
    window.index = new elasticlunr.Index;
    window.store = index_json;
    index.saveDocument(false);
    index.setRef('id');
    index.addField('url');
    index.addField('title');
    // add docs
    for (i in store) { index.addDoc(store[i]);}
    $('input#search').on('keyup', function () {
      var results_div = $('#results');
      var query = $(this).val();
      var results = index.search(query, {bool: "AND", expand: true});

      results_div.empty();

      for (var r in results) {
        var ref     = results[r].ref;
        var item    = store[ref];
        var link    = item.url;
        var title   = item.title;

        var result = '<div class="result"><a href="' + link + '"><p><span class="title">' + title + '.</span></p></a></div>';
        results_div.append(result);
      }
    });
});
```
