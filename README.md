# CVRA website
[![Build Status](https://travis-ci.org/cvra/cvra.github.io.svg?branch=master)](https://travis-ci.org/cvra/cvra.github.io)

See it live [HERE](http://cvra.github.io)

## Install dev tools

The only external requirement is Ruby>=2.0 and the `bundler` Gem (can be installed via `gem install bundler`).
To install all other needed tools by running `bundle install --path vendor --binstubs`.

## Running the dev version

Start the dev server by running `./bin/jekyll serve`.
The site automatically rebuilds on any change.


## Basic features include:

* SASS
* Responsive grid (Zurb Foundation)
* Responsive navigation
* Optional full-width banner

### GRID
Uses minimal sass [components](https://github.com/zurb/bower-foundation/tree/master/scss/foundation/components) from Zurb Foundation:

* [grid](http://foundation.zurb.com/docs/components/grid.html)


### BANNER
This theme is configured with a 'wrap' of 1920px so banner images look best at that width.

First it checks a pages yaml frontmatter for the header image, if none is found then it checks for a site-wide default in your config.yml, if none is found then no banner image is displayed.

**Site-Wide**
You can set a site-wide default banner image by adding the following to your _config.yml:
  `header_image: "path/to/image.jpg"`

**Per Page**
You can also override it per page by adding the following code to a pages yaml front matter:
  `header_image: path/to/image.jpg`
