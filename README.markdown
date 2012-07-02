# Scottish Ruby Conference 2012 - Edinburgh, UK

Notes and links to the talks I had the pleasure to hear.

It a rough version by now - links to the ScotRubyConf website, Speakerdeck, Lanyard will follow...

## Day 1
1. [Keynote by Mike Lee][day_1_0]
2. [Power Rake by Jim Weirich][day_1_1]
3. [Decoupling Persistence by Piotr Szotkowski][day_1_2]
4. [Hexagonal Raily by Matthew Wynne][day_1_3]
5. [Steven Baker - Maintainable Rails][day_1_4]
6. [Justine Arreche - I am a designer][day_1_5]
7. [Ben Orenstein - Refactoring from good to great][day_1_6]

## Day 2
1. [Keynote by Aaron Patterson][day_2_0]
1. [Someone is wrong - Joseph Wilk][day_2_1]
1. [Just open a Socket by Tyler McMullen][day_2_2]
1. [TDD your command line apps for fun and profit by David Copeland][day_2_3]
1. [Therapeutic Refactoring by Katrina Owen][day_2_4]
1. [Active Record Anti-Patterns by Ethan Gunderson][day_2_5]
1. [Keynote by Dave Thomas][day_2_6]


### Mike Lee [@bmf](https://twitter.com/@bmf) - Diversity, 1st Keynote [day_1_0]

![(Notes)](https://twitter.com/LS_nerdherder/status/218696796568944642/photo/1/large)

***

### Jim Weirich [@jimweirich](http://www.twitter.com/@jimweirich) - Power Rake [day_1_1]

gitimmersion.com

FileList is an Array of Filenames - it's lazy loaded

rake 'file task' is different from normal task
don't follow rake namespace rules with file tasks
new file only created if not existing, or 'out-of-date'

```ruby

    require 'rake/clean'

    # Examples:
    # MYFILES = FileList[ pattern, pattern ]
    # MYFILES.include('images/**/*.png')
    # MYFILES.exclude('*.thumb*')

    # THUMBFILES = MYFILES.pathmap( pattern )
      # images/gem.png --> "thumbs/%n-thumb%x" --> thumbs/gem-thumb.png

    # to convert all png images in all subdirectories under images/
    MYFILES = FileList['images/**/*.png']
    # substitutes dirname images with thumbs (pathmap is not lazy loaded)
    THUMBFILES = MYFILES.pathmap("%{^images,thumbs}d/%n-thumbs%x")

    CLEAN.include(THUMBFILES, "thumbs")   # call 'rake clean' to delete all the THUMBFILES and all files in directory thumbs
    CLOBBER.include("final.png")          # does everything CLEAN does + deletes final.png

    directory "thumbs"    # creates this directory

    # This will build a file task for every image file - beware of large numbers
    THUMBFILES.zip(MYFILES).each do |target , source|
      containing_dir = target.pathmap("%d")
      directory containing_dir
      file target => [containing_dir, source] do
        sh "convert ..."
      end
    end

    file "final.jpg" => THUMBFILES do
      sh "convert #{THUMBFILES} -append final.png"
    end

    task :convert => "final.jpg"

    # call with "rake convert" - won't run again, if filedate of final.png newer than that of all thumbnails

```

An alternative version can be written with rules and lambdas

***

### Piotr Szotkowski [@chastell](https://twitter.com/@chastell) - Decoupling Persistence [day_1_2]

Many examples with Addresses
Book: Person Names around the World

NullDB - Library to stub the DB

Decorators
  Draper
  SimpleDelegator <-- check this out *

Composition over Inheritance
  Forwardable

AOP Aspect-Oriented-Programming
 RCapture (Logging library)
 Make explicit call to persistance methods - or route through persistence layer, every time you data was changed

DCI Data + Context + Interaction

Candy library to persist objects in MongoDB (genius or insane?)

***

### Matt Wynne [@mattwynne](https://twitter.com/@mattwynne) - Hexagonal Rails [day_1_3]
**(with Steve Tooke [@tooky](https://twitter.com/@tooky) and Kevin Rutherford[@kevinrutherford](https://twitter.com/@kevinrutherford))** 

* Why does software development get slower over time?
* modular development vs 'connected' development
* Using 'Metaphors' to design software - and to talk about software
* **Tell, don't ask!**


_Exerpt from the ScotRubyConf Website:_

<div class="content">
  <p>The things that make Rails great in the first few weeks of a new project are precisely what makes it hurt after a few months. Anyone who has worked on a medium-sized Rails app will have experienced pain like:</p>

<ul>
<li>High coupling, meaning you have to run all your tests all the time to check each change.</li>
<li>Slow tests.</li>
<li>Logic littered in view templates or helper modules.</li>
</ul>

<p>Changes get more and more expensive to make, and the fun grinds to a halt. How can you stop this from happening? And more importantly, how can you turn around a project that’s already hit this wall of pain?</p>

<p>You need to pull your app away from Rails.</p>

<p>In this practical talk, we describe <a href="http://c2.com/cgi/wiki?PortsAndAdaptersArchitecture">an architecture</a> for mature Rails applications where the framework becomes a <a href="http://confreaks.com/videos/759-rubymidwest2011-keynote-architecture-the-lost-years">plug-in</a> to your application. With hands-on demonstrations, you’ll learn how to define clear boundaries between your application’s domain and Rails’ domain. Now Rails can stick to doing what it does best &ndash; providing the persistence and HTTP stack &ndash; and your valuable business logic will be in <a href="http://blog.steveklabnik.com/posts/2011-09-06-the-secret-to-rails-oo-design">plain old</a> <a href="http://devblog.avdi.org/2011/11/15/early-access-beta-of-objects-on-rails-now-available-2/">Ruby objects</a> that are <a href="http://arrrrcamp.be/videos/2011/corey-haines---fast-rails-tests/">fast</a> to <a href="https://www.destroyallsoftware.com/screencasts/catalog/fast-tests-with-and-without-rails">test</a> and easy to reason about.</p>

***

### Steven Baker [@srbaker](https://twitter.com/@srbaker) - Maintainable Rails [day_1_4]

***

### Justine Arreche [@TheElefanta](https://twitter.com/@TheElefanta) - I am a designer (and so are you) [day_1_5]

Book: Bootstrapping Design

12 column grid
  take out groups of 2 for responsiveness
align shapes to grid

Color wheel
Colors stand for emotions (Color theory = some western psylogical stuff)
Contrasting vs Blending Colors
Color Scheme (the easy way): 100% - 50% - 30% - 0% (0 = white)
Color Scheme: Color 1 - Contrasting Color 2 - Color 3 in between 1 and 2 (on the wheel) - grey

Typography
Headlines ~ 24px (or more)
  Text ~ half size in px
    Copytext not that important ~ 9px

horizontal line spacing 22px (or more)

be reasonable about different browsers or special parts of the design - if it does not work in reasonable time, fuck it!

***

### Ben Orenstein [@r00k](https://twitter.com/@r00k) - Refactoring from good to great [day_1_6]

* run your tests
* temp vars --> private methods
* care for short argument lists
* if you have 2 or more variables that have to go together, create a dedicated DataType like Struct (or similar)
* naming is hard - but important
* **Tell, don't ask!**
* Just send an Object a Message!
* use Presenters = Decorators!

     @contact = contact || NullContact.new    # or delegate :name, :to => null_contact...

     class NullContact
      def name
        'no name'
      end

      # ...
    end


* Beware of "God Objects"
    * don't add any more LOC to those 2 or 3 classes, refactor them to become slimmer
    * how to identify them:
        * wc
        * high churn
        * buggy files

_Excerpt from ScotRubyConf website:_

<div class="content">
  <p>Most developers know enough about refactoring to write code that&rsquo;s pretty good. They create short methods, and classes with one responsibility. They&rsquo;re also familiar with a good handful of refactorings, and the code smells that motivate them.</p>

<p>This talk is about the next level of knowledge: the things advanced developers know that let them turn good code into great. Code that&rsquo;s easy to read and a breeze to change.</p>

<ul>
<li>Topics include:</li>
<li>The Open-Closed Principle</li>
<li>The types of coupling, and their dangers</li>
<li>Why composition is so damn great</li>
<li>A powerful refactoring that Kent Beck calls &ldquo;deep deep magic&rdquo;</li>
<li>The beauty of the Decorator pattern</li>
<li>Testing smells, including Mystery Guest and stubbing the system under test</li>
<li>The stuff from the last halves of Refactoring and Clean Code that you never quite got to :)</li>
</ul>

<p>These topics will be covered solely by LIVE CODING; no slides. We&rsquo;ll boldly refactor right on stage, and pray the tests stay green. You might even learn some vim tricks as well as an expert user shows you his workflow.</p>

</div>


***

### Aaron Patterson [@tenderlove](https://twitter.com/@tenderlove) - Rails Four, 2nd Keynote [day_2_0]

* Threading in MRI
    1. release GVL <---- check what implications come from that *
    2. do CPU operation (outside Ruby it will be done in parall!)
    3. acquire GVL (back to Ruby land)

* Rails Queue for an consistent API
* Streaming in Rails vs. directly opening the Sockets
* autoload vs eager loading to avoid deadlocks
* pre-load ALL the code on startup ?!

Fat Clients - Mobile / JavaScript

Rails.js (tbd) as a better consumer (than Backbone, Ember, ...)

> How to make a Salami

![(Notes)](https://twitter.com/jessabean/status/219404143935238144/photo/1/large)

***

### Joseph Wilk [@josephwilk](https://twitter.com/@josephwilk) - Someone is wrong [day_2_1]

> Knowing Rethoric will make you a better Programmer

The old greeks were reading aloud in the library - to not lose the ability to discuss.
Pathos - Logos - Ethos

Metaphors, Story telling, Passion in the delivery => to form a personal connection
Concede on a small issue -> make your point -> give evidence (past -> present -> future)

being wrong is easy - but also influenced by tense, timing and medium
be wrong in the right way (because the idea was wrong, not the form)

***

### Tyler McMullen [@tbmcmullen](https://twitter.com/@tbmcmullen) - Just open a Socket [day_2_2]

How to build a (better) distributed system

> What is your Message Paradigm? (As 'summary' of all important related questions.)

* Resque = Service - Message Queue - high level - not-general purpose
* RabbitMQ = Service - Multi-Paradigm - available - multi-purpose
* ZooKeeper = Service - Atomic Broadcast - available - non-general purpose
* ZeroMQ = Embedded - Multi-Paradigm - lower-level - Magic - broken community and code
* Sockets = Embedded - Multi-Paradigm - low level

*Future:* (as Tyler sees it)

* (entirely) peer-to-peer 
* minimal configuration
* self-sufficent nodes

Tyler wanted to introduce us to a new open source messaging system, but...
> ... it is hard to do it right - and you have to do it right!

***

### David Copeland [@davetron5000](http://www.twitter.com/@davetron5000) - TDD your command line apps for fun and profit [day_2_3]

> Problem: development environment = production environment

Script 'fullstop' should:
  * clone dotfiles from git
  * symlink to $HOME

use app "methadone" to bootstrap the new app (mkdir, create dirs for tests...)
app "aruba" ~ something with Cucumbers Given/When/Then ...

    # to write bash code in ruby style
    require 'fileutils'
    include FileUtils
    

> Do NOT test with your real $HOME dir
  
    before { set ENV['HOME'] = fake_home }
    after { set ENV['HOME'] = my_home }

_Excerpt from the website:_

<p>I&rsquo;ve been a programmer for over 15 years, and have cut my teeth on Java, but now write Ruby almost 100% of the time, along with a bit of Scala and other stuff.  I&rsquo;ve written a book on writing command-line applications in Ruby for the Pragmatic Programmers.</p>

<p>I love clean code, testing, making things work, doing it right, and being realistic about it.</p>

***

### Katrina Owen [@kytrinyx](http://www.twitter.com/@kytrinyx) - Therapeutic Refactoring [day_2_4]

write tests first
deleted unneeded stuff
extract parts into private methods (and clean them up)
clean up the rest

And have fun doing it - and be proud of the result.

Comments have be correct AND valuable

Clean up your dirty code!

Keep your tests fast - to optimize for developer happiness :)

***

### Ethan Gunderson [@ethangunderson](http://www.twitter.com/@ethangunderson) - Active Record Anti-Patterns [day_2_5]

Why care?
  Know your tools
  Maintainability

Leaky Abstractions
  .pluck(:id) <---- check this in the Rails 3.2 API
  change table(users, :bulk => true) <--- use bulk to change many columns in one table!!
  code in a way to avoid "Shotgun Surgeries"

Query Optimization
  Think of data as a stream
  Better use User.friends.find_each do (takes only 1000)
    or User.friends.find_in_batches (creates a scope)
    than User.friends.all do (finds all and creates all AR Objects in memory)
  ... Order.all.includes(&:user) ...??
  Check this gem to find problems: https://github.com/nesquena/query_reviewer

Race Conditions
 validates_uniqueness_of ... create unique_index to be safe - or run in a transaction

Callbacks
  avoid them
  or keep them really simple
  just do one thing
  only change the model itelf - NOT other models!

Law of Demeter
  delegate (even is this also does not feel great)

***

### Dave Thomas [@pragdave](http://www.twitter.com/@pragdave) - Don't label yourself, 3rd Keynote [day_2_6]

walking around in white sox
started with in 98 with Ruby 0.4
got a "Light field camera" from his wife

Labels
  Agile
  a Ruby Programmer
  ...
Labels are static, not wholy descriptive - but defines you, they don't live

practice **Ruby-do** (don't pronounce it Ruby-du - pronounce it Ruby-doh)
japanese character, meaning "Art of Movement"
  There are many ways, not only one.
  There is no "best" - just use the appropriate language/style
  "Good" code is not the only code.
  The only criteria for good code: can it be changed easily?
  care for the little things (clean code, alignement)
  think why you favor some language constructs over others
  practice - e.g. through coding katas
  Confidence comes from practice, not knowledge.
  Distrust:
    experts,
    "best practices"
    Labels

> You are what you do! - Do good!

![(Notes)](https://twitter.com/jessabean/status/219404886335434752/photo/1/large)

