
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

Day 2
=====
* Picked up Kristen from Canada Place.  This gave me time to go through the first 300 pages of Odersky's Scala book. 
* Okay, setting this up on a Windows computer... in Vagrant, so I don't have to deal with Windows poop.
* In order to make this work in Vagrant I needed to run `sudo apt-get install python-software-properties`
* And curl. Wow Vagrant is lightweight. 
* sbt isn't working. maybe if I recreate a skeleton project and run sbt from there? 

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
    /users/:username - all settings for user
    /users/:username/subscriptions - list of all subscriptions for user
    POST /users/:username/subscriptions/ - create new subscription
    /users/:username/subscriptions/:subscription - list of all ArticleStubs for one subscription
    DELETE /users/:username/subscriptions/:subscription
    PUT /users/:username/subscriptions/:subscription - rename or tag a subscription
    /users/:username/articles - top 100 unread ArticleStubs for user
    /users/:username/articles/:articlestub - one articlestub
    PUT /users/:username/articlestubs/:articlestub - Favourite or Read an ArticleStub
    /articles/:article - complete article

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
* And now it builds! Hello, Hello World application. 
* It's time to go out and have fun. GIT CHECKIN

DAY 4
=====
* Weekend of Wine, Women and Song was fun
* Okay, poking at the basic Unfiltered app.
* `GET(Path("/")) =>{ Html(<h1>Hello World</h1>) }` Not half bad.
* Okay, constructing some more complicated paths.
* Let's build some basic classes.  Feed and ArticleStub to start.
* Shit, the parameters can't be the same name as the fields of the class. That sucks.
* Oh, I can use parametric values to sort of end-run around that. 
* Okay, the first big task I think is going to be converting things into JSON.
* Let's try to import Argonaut.io.  Do I just add it to my build file and everything else happens through magic?
* Oh, I have to turn off sbt and turn it on again. Hey, downloading my dependencies! Neat!
* Argonaut.io's syntax is just... cryptic.
* lift-json looks easier. Maybe I'll try that. 

DAY 5
=====
* lift-json! Let's try that. First, we add it to build.sbt.
* `"net.liftweb" %% "lift-json" % "XXX"`
* No, that didn't work.  Oh, I have to replace XXX with the current version number.
* ... which is...
* not anywhere in the readme. What version are you? Maybe the version is one of the branches or tags?
* there are *dozens* of those.
* The sbt says it's looking for 2.9.2, let's try that. 
* According to StackOverflow, 2.9.2 is the version of _SCALA_ that I'm using, not the library version. Silly me.
* Okay.. uh.. well, the current version of lift is 2.5. Let's try that.
* `"net.liftweb" %% "lift-json" % "2.5"` works!
* Now it's time to learn to use their DSL.
* `("argle"->"bargle") ~ ("number"->32)` throws an error complaining that ~ isn't supported on Strings.
* Am I not including the right packages?
* After about 15 minutes of poking around:  I have to `include net.liftweb.json.JsonDSL._` as well, if I want
  access to the ~ operator.
* And now we have working JSON output! Hooray!
* Separating all of the classes into individual files... 
* And next, we have Authentication. 
* The GAE User page wants me to use 'getUserPrincipal' from the request object, but the
  scala request object doesn't have getUserPrincipal.
* is there some way for me to just dump the entire request object so that I can see it? 

DAY 6
=====
* We need to get some auth all up in here.
* When I try to use the UserServiceFactory, I get a `java.lang.NoClassDefFoundError`.  Boo! Hiss! 
* Okay, poking around the [Ant Build Instructions](https://developers.google.com/appengine/docs/java/tools/ant) it looks like 
   I was supposed to have been copying jars from the SDK into the WEB-INF/lib folder.  I'm surprised that this didn't come 
   as part of the prepackaged GAE+Unfiltered build. Oh well. 
* Yeah! The rest was a piece of pie! PIE!

DAY 7
=====
* What's next? On the wall I have Hello World (done), JSON (done), Authentication (done), Backend, RSS consume, Storage...
* I suspect that creating a backend to consume a RSS feed is the way to go. But it's friiiidaaaaaay nyeeeeh. 
* It looks like "Task Queues" might be more suited to my use case than Backends. 
* Which .. I just have a special address that processes an RSS feed? 
* Okay, standard run-of-the mill URL -> StreamReader -> BufferedReader Java bullshit. Let's abstract this into a fetcher. 
* Fetch.scala. Lazy little bit. 
* Before I forget, I should remember to set the MIMEType correctly on the JSON I'm sending back to the user. This isn't plaintext, bub.
* Okay, time to parse some RSS feeds. Are there any RSS libraries I can use?
* Looks like ROME is deader than dead, and feed4j was written in the Old Times. Nothing for Scala. Guess I'm building it myself.
* Writing a RSS reader that parses to spec is pretty damn easy, with Scala's built in XML tooling. 
* The problem with RSS parsing is... special cases. Oh so many special cases. Let's look at some!
 * RSS+Atom together compose about 11 competing and not entirely compatible protocols. 
 * Some Atom feeds have multiple `<link>` elements!
 * RSS2 uses `<link>http://...</link>` whereas Atom uses `<link href='http://...'>`, I think? 
 * The correct date format for RSS2 is RFC 822, but Penny Arcade mistakenly uses ISO 8601 instead
 * Travis's feed (http://travisbrown.ca/blog.rss) uses both `<description>` and `<content>` fields. 
* The Syndicate code might just need a suite of Unit Tests to work through. It's certainly going to be complicated enough. 
* But just being able to parse basic RSS gets us at least a little bit of the way there. LITTLE VICTORY! 

DAY 8
=====
* Okay, trying to run this on my laptop again. Everything goes smoothly except... 
* It's listening on 127.0.0.1, when it needs to be listening on 0.0.0.0
* Shit, how do I change that setting? It's gotta be somewhere in the sbt config, right? 
* [This might help.](https://github.com/sbt/sbt-appengine/blob/master/src/main/scala/AppenginePlugin.scala)
* Oh, so I can just pass the settings right to sbt. Nice.
* `appengine-dev-server --address=0.0.0.0`

DAY 9
=====
* Okay, in order to get the RSS reader working the way I want it to, I'm going to engage Many Unit Tests. 
* We'll keep a file full of RSS Examples, and then check to make sure that the examples all parse properly.
* Using scalatest rather than this Specification thing. BDD bleugh. 
* WHY WON'T IT FIND THE TESTS? RAARGH.
* Oh, I'm trying to run sbt from `potater` rather than `potater/potater`. I _knew_ that would be confusing. 
* Tests running. Let's write some! 
* Let's get vim-scala style running properly on my laptop. 
* Okay, neocomplcache is awful. Do not want. 
* Joda Time! First instinct was to install the `nscala_time` helper library, but an undocumented wrapper
    around Joda is worse than just using (well-documented) Joda. 
* We need to format pretty arbitrary dates. The way to do this in Joda is to
    construct a Formatter chain containing all possible formats and take a 
    crack at it. 
* Seems to.. work? 
* Okay, I'm not getting link elements properly. Atom feeds can have multiple link
    elements, each with a href, and I want the one that doesn't have a 'rel', I think.
* How do I select only elements that don't have a "rel" defined? 
* Okay, I think I've got it. 
* Woohoo, I've got all of my unit tests passing. Let's write more! 
