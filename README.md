
DAY 0
=====
* What? They're discontinuing TheOldReader? I'm not invited? Well then I'll build my OWN reader.
* But with blackjack and hookers. 
* In fact, forget the reader.
* Okay, what should I build it in? It's a side project, so I should pick something I don't know how to use.
* Either Scala or Node.js
* Well, Scala looks stable, speedy, well-respected, concurrency-friendly, and well-written. 
* On the other hand, Node.js _is_ trendy and trend-whoring hasn't been bad for my career SO far.
* Eh, Scala, for now. I'll prostrate myself before the altar of Javascript some other time. 
* Channel says that OpenJDK is terrible. Okay, keeping that in mind.
* I remember Kyle used Scalatra for a project of his. Oh, this looks neat. I'll try this.
* Scribble some DB design on a napkin. 
 * User 
 * Feed
 * UserSetting (FK User)
 * Subscription (FK Feed)
 * ArticleStub (FK Feed)
 * Article (FK ArticleStub)
 * ArticleStatus (FK ArticleStub, FK User)

DAY 1

=====
* Flipped through the Scala chapter of Seven Languages in Seven Weeks at high speed. I'll learn as I go. 
* [Installation](http://www.scalatra.org/getting-started/installation.html)
* What's an Akka Actor? I read about Actors in Seven Languages, but I've never heard of an Akka Actor. is it the same thing? 
    More investigation later.
* [Java 7 Oracle](http://www.webupd8.org/2012/01/install-oracle-java-jdk-7-in-ubuntu-via.html)
* I wonder if I have any domains lying around that I could use for this. Okay, lasercake.com and potater.com. 
* Let's call it potater, then.
* Ugh, my source code is in /src/main/scala/net/lassam/potater . I remember hating this about Java. 
* Is Vim not highlighting Scala properly? Is.. is that not a thing that Vim can do? Aggh. 
* [Editing Scala with vim](http://leonard.io/blog/2013/04/editing-scala-with-vim/)
* Okay, which means I need to start with [vim-scala](https://github.com/derekwyatt/vim-scala), which is a vim bundle,
    which means I have to install [Pathogen](https://github.com/tpope/vim-pathogen), first.
* Oh, while I'm doing that, I should install a [markdown plugin](https://github.com/plasticboy/vim-markdown) for vim. 
* Should I also install [neocomplcache](https://github.com/Shougo/neocomplcache.vim)? I don't know, I don't like dicking around with Vim too much...
* I'll leave more vim configuration for later. Back to Scalatra. 
* Okay, let's run this bad boy. ./sbt, then container:start
* Let's see what I'm running on localhost:8080.
* .. Jenkins. Shit. I'm already running something on 8080. If memory serves me, I already have dev stuff running on 8000, 9000, 8080... 
* Okay, can I tell sbt to run on a different port?
* [Here's what I should do.](http://www.scalatra.org/guides/deployment/configuration.html#toc_189).
* Ugh there is so much crufty Java bullshit in here. Maybe I _should_ have gone with node.js
* It tells me I should add the line `port in container.Configuration := 8081` to `project/build.scala`, but it doesn't tell me where. 
* I've never seen a pattern like  `x in y := z` before. I wonder what that means. 
* ... let's try at the end of the file, before the closing brace.


    [info] Loading project definition from /home/classam/code/potater/project
    [info] Compiling 1 Scala source to /home/classam/code/potater/project/target/scala-2.9.2/sbt-0.12/classes...
    [error] /home/classam/code/potater/project/build.scala:46: not found: value port
    [error]   port in container.Configuration := 8222
    [error]   ^
    [error] one error found
    [error] (compile:compile) Compilation failed


* Okay, so not there. Let's look a little closer at this file. 
* There are some symbols in here that I just plain do not understand. %%, :=, ++=... what the shit is this? 
* Googling operators is impossible, dammit! Let's just google 
    "[Scala operators](http://www.tutorialspoint.com/scala/scala_operators.htm)" and see the whole list.
* No, this is just standard C-style operators. I still don't know what the hell a := or a %% or a ++= does.
* Does scala support operator overloading? If it does I am going to throw a _fit_.
* [Oh, shit, it does.](http://stackoverflow.com/questions/1098303/what-makes-scalas-operator-overloading-good-but-cs-bad)
* Okay, let's try putting the line here, at the end of the Settings block. That seems like a good spot. And I'll need a comma, here, too.


    [error] /home/classam/code/potater/project/build.scala:44: not found: value port
    [error] Error occurred in an application involving default arguments.
    [error]       port in container.Configuration := 8222
    [error]       ^
    [error] one error found
    [error] (compile:compile) Compilation failed


* _Nope._
* Default arguments? Maybe the := symbol has something to do with default arguments. [To Google!](http://www.scala-lang.org/old/node/2075) Nope.
* Shit, it would be easier to just move Jenkins at this point. No, I should figure this out. Let's look more closely at this. 
* We're instantiating a Project class. With the 'lazy' keyword. That's new, [what does that mean?](http://stackoverflow.com/questions/9809313/scalas-lazy-arguments-how-do-they-work). Oh. Neat. 
* We're passing in the name 'potater', a file reference to '.', and.. a named variable, 'settings', which we're setting to Defaults.defaultSettings ++ some other things.
* So. contextually, ++ is probably some sort of list concatenation? We're ++-ing this with a Seq, so maybe ++ means Sequence Concatenation, then? 
* [Bingo!](http://www.scala-lang.org/api/current/index.html#scala.collection.Seq). That's one wierd operator down. 
* But why a Seq? It looks like just a run-of-the-mill ordered list. Why not use a List instead?
* I'm still kind of stumped as to how the Seq contains all of these := pairs. 
    Are they tuples or something? Let's google search [scala project build.scala](http://www.scalatra.org/2.2/getting-started/project-structure.html)
* Oh, hey, "SBT" stands for "[Simple Build Tool](http://www.scala-sbt.org/)". Coolbeans.
* Nope, this isn't helping. Okay, I'm just going to turn off Jenkins. I wasn't using it anyways. 
* There, take that, Jenkins. Port 8080 is all clear. 
* Aaaand there we go, a Hello World app. 
* But at WHAT COST. 
* What I wouldn't give for settings.py and Django's command line management tool right now. 
* Okay, and... I'm done the tutorial? Seriously? That's all you got? Well, fuck. 
* I want to check this in to git, but I just did a compilation step. How do I clean out the compiled files? Is there a `sbt clean`?
* .. apparently there is. I wonder if it did anything?
* well, if compiled files went anywhere in this heirarchy, I bet they'd be in 'target'.  It's empty? Okay, good.




DAY 3
=====
* Okay, sbt wasn't working on the other computer. I wonder if channel knows why.
* Travis and Kyle together solve the problem for me. I didn't check in the hidden .lib directory, containing the vital sbt-launch.jar
* Kyle recommends Unfiltered, too. (Jon had recommended that earlier.) Should I switch? NO SWITCHING HORSES IN MIDSTREAM. 
* I should look at maybe deploying this on Google App Engine.
* Apparently scalate has some gotchas when deploying with GAE - no temp space, need to precompile the templates before deployment. Check. 
* Also 'uploads'. 
* I'll need their SDK for that, and to do more reading. 
* Should I use their Cloud SQL? Or deploy on App Engine Datastore? Maybe BOTH bwa ha ha ha ha
* Okay, all of this speculative stuff is getting out of control. Let's start by just figuring out the API that I want to serve.

    /users - touchpoint for auth stuff or redirect to /users/:username
    /users/:username/subscriptions - list of all subscriptions for user
    POST /users/:username/subscriptions/ - create new subscription
    /users/:username/subscriptions/:subscription - list of all ArticleStubs for one subscription
    DELETE /users/:username/subscriptions/:subscription
    PUT /users/:username/subscriptions/:subscription - rename or tag a subscription
    /users/:username/articlestubs - top 100 unread ArticleStubs for user
    /users/:username/articlestubs/:articlestub - one articlestub
    PUT /users/:username/articlestubs/:articlestub - Favourite or Read an ArticleStub
    /users/:username/favourites - articles Favourited by the user
    /articles/:article - complete article
    /feeds/:feed - canonical data on a single feed

    and some core objects are going to be 'User', 'ArticleStub', 'ArticleStatus', 'Article', 'Feed', 'Subscription' 

* that's a start, okay. /users. Let's.. read about Scalatra auth and GAE auth.
* GAE auth is just "Google Accounts", which is fine by me. I have one of those.
* I have to import stuff from the google libraries to use it. Maybe getting to Hello World with Scalatra+GAE tools is next step. 
* Oh, mother FUCK, Scalatra and GAE don't play nice together. 
* Getting real tired of this shit. Still haven't written any damn code. 
* Scalatra docs: "Here's the simplest possible case. If you want to do anything more complicated, why not just look at the source code?"
* Unfiltered looks pretty nice, AND it works with GAE. Interesting. 
* Let's try creating a basic Unfiltered project using the GAE template...
* Oh, I need to install sbt this time, it doesn't come with the project. I guess I could just move sbt-launch.jar to my /bin folder...
* [Here](http://www.scala-sbt.org/release/docs/Getting-Started/Setup.html) is a handy guide for that. 
* It won't build. 

    [error] You need to set APPENGINE_SDK_HOME
    [error] Use 'last' for the full log.

* I feel like I need to download the Google App-Engine SDK and then set an environment variable pointing to its location. 
* Oh, the g8 [comes with documentation](https://github.com/unfiltered/unfiltered-gae.g8).
* it tells me to do exactly what I figured I'd need to do.
* It's time to go out and have fun. GIT CHECKIN
