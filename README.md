# Phil

Phil is a lightweight content generation module that wraps around [Ffaker](https://github.com/EmmanuelOga/ffaker/tree/master/lib/ffaker). You can create large amounts of markup for layout testing with a few simple methods.

A big theme of Phil is that any parameter that can accept a number also accepts a range. This allows for far more utility than vanilla Ffaker when it comes to testing different permutations of content.

## Iteration

Get a random integer from an array or range:

```ruby
Phil.pick 1..3
Phil.pick [1, 2, 3]
Phil.pick ["Foo", "Bar", "Baz"]
```

Loop a random number of times:

```ruby
Phil.loop 1..100 do |i|
  "This will be output between 1 and 100 times and is index #{i}"
end
```

Have a 1 in N chance of doing something (N defaults to 3):

```ruby
Phil.sometimes "foo"                    # 1 in 3
Phil.sometimes "foo", 100               # 1 in 100
Phil.sometimes do
  "foo"
Phil.sometimes 100 do
  "foo"
```

## Content Generation

### Body content

Generate a ton of body content with one method. This defaults to
`"h1 p p h2 p ol h2 p ul"`, but you can pass it a string of whatever content tags you like,
including `blockquote`, other headings, and so on. Blockquotes will contain multiple `<p>` elements, paragraphs will contain a few sentences, and any other tags will contain 3-15 words.

```ruby
Phil.markup
Phil.markup "h1 p h2 ul p blockquote h5 h6"
```

### Images

Generate a [placehold.it](http://placehold.it) image URL. Requires dimensions, but you can also request colors (in the format `#background/#foreground`) and containing text. Placehold.it doesn't play nice with some containing text characters so those are stripped out.

You can pass parameters in any order if you like – Phil is reasonably smart about figuring them out – or name them if you're feeling verbose.

```ruby
Phil.image 200                          # http://placehold.it/200
Phil.image '200x400', '#ff0000'         # http://placehold.it/200x400/ff0000
Phil.image '200x400', '#ff0000/#00ff00' # http://placehold.it/200x400/ff0000/00ff00
Phil.image 'Jackie Jormp-Jomp?', 600    # http://placehold.it/600&text=Jackie+Jormp-Jomp

# or pass named parameters (you can use ranges for size here)
Phil.image(text: 'Pants', width: 300, height: (300..500), background: '#ff0000', foreground: '#0000ff')
```

### Lorem methods (all take ranges or numbers)

```ruby
Phil.words 5
Phil.words 5..50

Phil.paragraphs 5                       # outputs HTML markup with <p> elements
Phil.paragraphs 5..50

Phil.blockquote                         # defaults to containing 1..3 <p> elements
Phil.blockquote 1..5                    # 1..5 <p> elements

Phil.ul                                 # defaults to 3..10 <li>s with 3..15 words
Phil.ul 1..5, 10                        # 1..5 items, 10 words apiece

Phil.ol
Phil.ol 1..5, 10

Phil.link_list                          # outputs a <ul> of <li>s with <a>s inside
Phil.link_list 1..5, 10
```

### Assorted other content generation methods

```ruby
Phil.date                               # Random date object between Dec 31 1969 and now
Phil.date 7                             # Random date object in the last 7 days
Phil.date 7-14                          # Random date object in the last 7-14 days
Phil.currency 10..100                   # Price string from $10.00 to $100.99
Phil.currency 10..100, "£"              # Prefix different currency symbol
Phil.phone                              # Defaults to (###)-###-####
Phil.phone "###-#### x###"
Phil.number 5                           # Random 5-digit number (string)
Phil.number 5..10                       # Number 5-10 digits long
```

### Aliased Ffaker methods for convenience

```ruby
Phil.address
Phil.city
Phil.state_abbr
Phil.domain_name
Phil.email
Phil.name
Phil.first_name
Phil.last_name
Phil.company_name
```
