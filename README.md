# OBJECSS

## A design-free, modern, Responsive, BEM-inspired, OO, Sass-based framework

* **Sass** based, `.scss` syntax
* **Modern**: `display: flex`, `calc()`, `rem`-values, etc. with no fallbacks for unsupporting browsers
* **Modular**: Seperate OOCSS components. (En/dis-able in `./main.scss`)
* **Unprefixed**: They'r added by [Autoprefixer](https://github.com/postcss/autoprefixer)
* **BEM-based**: `component-name__sub--modifier` convention (configurable)
* **Mobile first**: basic mobile-first responsivity
* **Design-free** structural styles mainly. BYO `./theme/_my-theme.scss`

## TODO

- Implement http://sassdoc.com/
- Look at https://github.com/brigade/scss-lint (and ST plugin)

### SYNTAX

Generally check [sass-guidelin.es](http://sass-guidelin.es/) and [cssguidelin.es](http://cssguidelin.es/).
However: **consistency** is most important.

I've added a [`.editorconfig`](http://EditorConfig.org) and [`.scss-lint.yml`](https://github.com/brigade/scss-lint)

- 2 spaces (no tabs)
- space before `:` (`padding-top: .5em;`) and after `,`: (`rgba(0, 0, 0, .4)`)
- Multi-line CSS rules
- 'Single quotes' around all strings and URL's
- No leading (`0.5em`) and trailing (`1.0em`) zero's (use: `.5em` and `1em`)
- No unit for `0`-values (not `padding: 0em;`, but `padding: 0;`)
- etc. etc.

### Comments

Use consistent [DSS](https://github.com/darcyclarke/DSS) parsable comments with `@name`, `@description`, `@states` and `@markup`:

```
/**
  * @name Button
  * @description Your standard form button.
  *
  * @state :hover - Highlights when hovering.
  * @state :disabled - Dims the button when disabled.
  * @state .primary - Indicates button is the primary action.
  * @state .smaller - A smaller button
  *
  * @markup
  *   <button>This is a button</button>
  */
 button {
   ...
 }
 ```

You can [auto-complete these in ST3](https://github.com/sc8696/sublime-css-auto-comments)

**Inline comments** use the `// comment` style.
To allow quickly jumping to a section you could add certain placeholders:

```
/**
 * =1 Section One
 */
```

### Naming conventions

I try to adhere to a generic [OOCSS](http://oocss.org) approach. Much inspiration from (and many thanks to) [SMACSS](http://smacss.com), [BEM](http://bem.info), [INUIT CSS](http://inuitcss.com/)

#### Classnames

* **Component:** `.component-name` -› `.media`
* **Sub-object:** `.component-name__sub-object` -› `.media__img`
* **Modifier:** `.component-name__sub-object--modifier` -› `.media__img--reversed`
* **State:** `.is-some-state` -› `.is-active`, `.is-hidden`
* **Utilities:** `.u-utility-name` -› `.l-center`
* **Layout helpers:** `.l-helper` -› `.l-1of2`

Grid- and grit units are the *exception*: `.grid` and `.grid__unit` (not prefixed)

I'm on the fence re: Javascript selectors. I like to use `data-` attr. for these but one could also use `.js-` prefixed classnames. As long as it's *consistent*.

#### Sass Variables

Most Sass variables are prefixed so that one knows what type to expect:

* `$cl-*` **Color value:** * `$cl-black`, `$cl-accent`, `$cl-link-hover`
* `$ff-*` **Font family value:** * `$ff-body`, `$ff-headings`
* `$fs-*` **Font size value:** * `$fs-base-px`, `$fs-base-pc`
* `$ln-*` **Line- value (height:unitless):** * `$ln-height`
* `$pd-*` **Padding value:** * `$pd-default`
* `$mg-*` **Margin value:** * `$mg-default`
* `$bd-*` **Border value:** * `$bd-radius`
* `$mq-*` **Media Query:** * `$mq-narrow`, `$mq-medium`, `$mq-wide`

Colors are both named after their *color* (`$cl-grey`) and after their *use* (`$cl-border`). This allows us to do:

````
$cl-grey: #CCC;       // No need to remember #CCC
$cl-border: $cl-grey; // .. but using $cl-border in components
````

#### Folder Structure

`main.scss` is the *only* compiled file. The rest is `@import`ed from there.
Print styles are added at the end of `main.scss`

```
objecss-scss/
|
|– base/                  ### ELEMENT-level styling
|   |– _sanitize.scss     # Reset/normalize
|   |– _webfonts.scss     # Webfont declarations (Woff, Woff2)
|   |– _document.scss     # Global HTML/BODY styles
|   |– _hyperlinks.scss
|   |– _type.scss         # Paragraphs, Headings, Quotes, etc
|   |– _lists.scss        # (un)-ordered, definition
|   |– _images.scss       # Images, Figure
|   |– _tables.scss
|   |– _forms.scss
|   ...                   # Etc…
|
|– components/            ### COMPONENTS (OOCSS)
|   |– _button.scss       # Buttons
|   |– _media.scss        # Media Component
|   |– _nav.scss          # Navigation Component
|   ...                   # Etc…
|
|– layout/                ### LAYOUT styles (page-layout mainly)
|   |– _general.scss      # Generic layout styles
|   |– _grid.scss         # Grid system
|   ...                   # Etc…
|
|– pages/                 ### PAGE SPECIFIC
|   |– _home.scss         # Home specific styles
|   ...                   # Etc…
|
|– themes/                ### THEMING
|   |– _theme.scss        # Default theme
|   |– _custom.scss       # Custom theme
|   ...                   # Etc…
|
|– utils/                 ### UTILITIES (should NOT output any CSS!)
|   |– mixins/            # Folder containing Mixins
|   |–- _mixin.scss       # - Some mixin
|   |-- ...               # Etc…
|   |– functions/         # Folder containing Functions
|   |–- _function.scss    # - Some function
|   |-- ...               # Etc…
|   |– _settings.scss     # Sass Variables
|   |– _mixins.scss       # Sass Mixins (en/disable here import from folder)
|   |– _functions.scss    # Sass Functions (en/disable here import from folder)
|   |– _helpers.scss      # Class & placeholders helpers
|   |– _debug.scss        # Diagnostic styles such as vertical rhytm helpers, outlines
|
|– print/                 ### PRINT STYLES
|   |– _print.scss
|
|– vendors/               ### THIRD PARTY VENDOR STYLES
|   |– _some-library.scss # Some 3d-party Library UI
|   ...                   # Etc…
|
|
`– main.scss              # Main Sass file
```

To make best use of the cascade `main.scss` imports files in the following order:

01. ./vendors/xyz.css
02. ./utils/_settings.scss
03. ./utils/_functions.scss
04. ./utils/_mixins.scss
05. ./base/_xyz.scss
06. ./utils/_helpers.scss  # After Base because these contain helper classes e.g. clearfix
07. ./layout/_xyz.scss
08. ./components/_xyz.scss
09. ./pages/_xyz.scss
10. ./themes/_xyz.scss
11. ./print/_print.scss
12. ./utils/_debug.scss

