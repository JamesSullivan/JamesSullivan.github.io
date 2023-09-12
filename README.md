# [james.sullivan at github.io](https://JamesSullivan.github.io)


[![Jekyll](https://img.shields.io/badge/jekyll-%3E%3D%203.8-blue.svg)](https://jekyllrb.com/)

A simple [bootstrap](https://getbootstrap.com/) based jekyll theme for my former homepage/blog (archived).

The site uses
<img src="https://raw.githubusercontent.com/JamesSullivan/JamesSullivan.github.io/gh-pages/assets/images/lwt-logo-wolf-v2.svg" alt="isolated" width="50"/>
- [LWT](https://github.com/manid2/lone-wolf-theme)
- [github-pages compatible gems](https://pages.github.com/versions/)
- [bootswatch wrappers](https://bootswatch.com/)
- [animate.css](https://daneden.github.io/animate.css/)

## Instructions

[https://jekyllrb.com/docs/](https://Jekyllrb.com/docs/)

[https://github.com/manid2/lone-wolf-theme/#readme](https://github.com/manid2/lone-wolf-theme/#readme)

Requires Ruby and Jekyll installed. Then clone repo and cd in

```bundle add webrick  # only needs to be done once```

```bundle exec jekyll serve --livereload```

Browse to [http://localhost:4000/JamesSullivan.github.io.git/](http://localhost:4000/JamesSullivan.github.io.git/)


<br>

### Importing Blogger

If you are importing a blog from blogger
see [https://import.jekyllrb.com/docs/blogger/](https://import.jekyllrb.com/docs/blogger/) to install appropriate gems, then run the following Ruby script.

```ruby
ruby -r rubygems -e 'require "jekyll-import";JekyllImport::Importers::Blogger.run({"source" => "blog-01-16-2023.xml","no-blogger-info" => false, "replace-internal-link" => true, "comments" => true })'
```
