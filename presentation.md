# [fit] Hello
# My name is @folone

![](https://cdn.eyeem.com/thumb/900/900/875ca603aa93985dfba529d82407d198ef9cad3d?highRes=true)

https://github.com/folone/scalaworld

---

![](https://dl.dropboxusercontent.com/u/4274210/sc_cmyk_white.ai)

---

![fill inline](https://i1.sndcdn.com/avatars-000106773882-y556ey-t500x500.jpg)![fill inline](https://i1.sndcdn.com/artworks-000037484402-x8gfft-t500x500.jpg)![fill inline](https://i1.sndcdn.com/avatars-000138955484-r3yxsu-t500x500.jpg)![fill inline](https://i1.sndcdn.com/avatars-000114613706-sva2ir-t500x500.jpg)
![fill inline](https://i1.sndcdn.com/avatars-000109087465-2uqbx8-t500x500.jpg)![fill inline](https://i1.sndcdn.com/avatars-000055035285-1i17eh-t500x500.jpg)![fill inline](https://i1.sndcdn.com/avatars-000144828163-4o5nir-t500x500.jpg)![fill inline](https://i1.sndcdn.com/avatars-000043925604-s4mhaz-t500x500.jpg)

^ todo: something in the background?

---

* 12 hours uploaded every minute
* ~35k listening years every month
* >100M tracks
* ~300M monthly active users

![left](https://cdn.eyeem.com/thumb/900/900/72a3ad530c5b60e4b6077536aecce56cbdf526c0?highRes=true)

^ quite unique
^ 4x spotify audio (>1PB vs >5PB)
^ 10x smaller team than spotify
^ a lot more interactive, as in the next slide
^ by a huge margin bigger than any competitor in both usage and amount of data (collection size)
^ except for youtube: that's bigger, but they aren't in the audio business per se

---

# unexpected challenges<br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br>

![](https://s3.amazonaws.com/f.cl.ly/items/0L1T380N1X3l262F3n3q/Image%202015-08-28%20at%203.51.58%20pm.png)

^ war stories, we have some fun challenges due to the social nature of soundcloud
^ Drake vs Meek Mill

---

# hands on
# [fit] TLC
:ghost:

![](https://cdn.eyeem.com/thumb/900/900/e0f90308f1176ae636211754da263e6503d1d3bd?highRes=true)

---

# [fit] T-L-_what now?_[^0]
# [fit] *ಠ_ಠ*

![](https://cdn.eyeem.com/thumb/900/900/cb2220a35cd33c137a8e096e6471550253d80fe0?highRes=true)

[^0]: http://typelevel.org/blog/2014/09/02/typelevel-scala.html

^ TLC -- short for typelevel compiler. Name actively endorsed by Daniel Spiewak.
^ A scala fork made by the typelevel organization, to scratch an itch.
^ Typelevel Scala and the future of the Scala ecosystem

---

# [fit] Why we love ![Scala](http://www.scala-lang.org/resources/img/smooth-spiral@2x.png)

:ghost:

[Roll your own Scala](https://meta.plasm.us/posts/2015/07/11/roll-your-own-scala/)

![](https://cdn.eyeem.com/thumb/900/900/dba3443e5728917676bece667743d0762005145e?highRes=true)

^ To recap: we’ve taken a few basic (but still pretty broken) Scala language features — singleton types, refinement types, type projections, and the implicit resolution system — and we’ve very painfully built a language for ourselves that at least kind of looks like it supports partial type parameter application, higher-order unification, and multiple implicit parameter sections.
 

---

# [fit] I think that’s pretty neat,
# [fit] but I can also understand why
# [fit] almost everyone else would find it
# [fit] horrifying.

-- Travis Brown

![](https://cdn.eyeem.com/thumb/900/900/934c62c288e50e609e02e11d584f3b8ea6651c78?highRes=true)

^ This is not what makes people come to scala
^ but it is what makes some of them stay with it

---

# [fit] TLC
:ghost:

![](https://cdn.eyeem.com/thumb/900/900/c0fb69ecda06f5d9a5ed5fed5093698b71b7b3fe?highRes=true)

^ For the people who miss pre 2.10 days
^ An incubator for new ideas
^ Beneficial for typesafe scala: they get battle-tested contributions
^ good news: scala will soon be perfect because of The Third System Effect
^ Note: we aren't compiler devs yet, we have full time jobs, and really don't know what we're doing. So don't take anything here as a promise or a final decision or anything that has any level of commitment. The whole point of this talk is to inspire collaboration.
^ Also don't rely on our support for the same set of reasons
^ Backwards compatibility with typesafe scala

---

# [fit] scalaVersion        := "2.11.7"
# [fit] scalaOrganization   := "org.typelevel"

```sh
∙ git clone git@github.com:folone/scalaworld.git && \
  cd scalaworld && \
  sbt -Dsbt.boot.properties=sbt.boot.properties

[repositories]
  maven-central
```

^ sbt is awesome, works starting from 0.13.6
^ jcenter has a version from mid August, which has a bug
^ ONCE
^ 2.11.8-SNAPSHOT is published as well

---

# [some] Features

- Type lambdas
- `@implicitAmbiguous` (coming to 2.12 #4673)
- Singleton types
- -Zirrefutable-generator-patterns
- Nifties

![left](https://cdn.eyeem.com/thumb/900/900/4c78f4df5242de6c55a02a8bad5345b766e66463?highRes=true)

---

# Type lambdas

```scala
trait Functor[F[_]] {
  def map[A, B](fa: F[A])(fn: A => B): F[B]
}

trait LeftFunctor[R] extends
  Functor[({type U[x] = Either[x, R]})#U]

trait RightFunctor[L] extends
  Functor[[y] => Either[L, y]]
```

---

# Type lambdas

```scala
[x] => (x, x)
[x, y] => (x, Int) => y
[x[_]] => x[Double]
[+x, -y] => Function1[y, x]
```

---

# [fit] Type lambdas
# [fit] are cool and all,
# [fit] but not a single line
# [fit] of the compiler was ever
# [fit] written with them in mind
-- Paul Phillips (SI-6895)

![](https://cdn.eyeem.com/thumb/900/900/615f30b66478a6218aa3439bad83f5be9427481c?highRes=true)

---

# `@implicitAmbiguous`[^1]

```scala
// Encoding for "A is not a subtype of B"
trait <:!<[A, B]
 
// Uses ambiguity to rule out the cases we're trying to exclude
implicit def nsub[A, B] : A <:!< B = null
@typelevel.annotation.implicitAmbiguous("Returning ${B} is forbidden.")
implicit def nsubAmbig1[A, B >: A] : A <:!< B = null
implicit def nsubAmbig2[A, B >: A] : A <:!< B = null
 
// Type alias for context bound
type |¬|[T] = {
  type λ[U] = U <:!< T
}
 
```

[^1]: https://gist.github.com/milessabin/c9f8befa932d98dcc7a4

---

# `@implicitAmbiguous`

```scala
def foo[T, R : |¬|[Unit]#λ](t: T)(f: T => R) = f(t)
 
foo(23)(_ + 1)   // OK
foo(23)(println) // Doesn't compile: "Returning Unit is forbidden."
```

---

# Singleton types[^2]

```scala
trait Assoc[K] { type V ; val v: V }
 
def mkAssoc[K, V0](k: K, v0: V0): Assoc[k.type] { type V = V0 } =
  new Assoc[k.type] {type V = V0 ; val v = v0}
def lookup[K](k: K)(implicit a: Assoc[k.type]): a.V = a.v
```

[^2]: Requires `-Xexperimental` flag.

^ Adriaan's implementation

---

# Singleton types

```scala
implicit def firstAssoc = mkAssoc(1, "Panda!")
//> firstAssoc : Assoc[Int(1)]{type V = String}
implicit def secondAssoc = mkAssoc(2, 2.0)
//> secondAssoc : Assoc[Int(2)]{type V = Double}

implicit def ageAssoc = mkAssoc("Age", 3)
//> ageAssoc : Assoc[String("Age")]{type V = Int}
implicit def nmAssoc = mkAssoc("Name", "Jane")
//> nmAssoc : Assoc[String("Name")]{type V = String}
```

---

# Singleton types

```scala
lookup(1)
// > res1: String = Panda!
lookup(2)
// > res2: Double = 2.0
lookup("Age")
// > res3: Int = 3
lookup("Name")
// > res4: String = Jane
```

---

# Irrefutable generator patterns

```scala
  for {
    (x, _) <- Option((1, 2))
  } yield x
```

---

# Desugars to

```scala
Some(scala.Tuple2(1, 2))
  .withFilter(((check$ifrefutable$1) =>
    check$ifrefutable$1: @scala.unchecked match {
      case scala.Tuple2((x @ _), _) => true
      case _ => false
    })).map(((x$1) => x$1: @scala.unchecked match {
      case scala.Tuple2((x @ _), _) => x
    }))
```

---

# With irrefutable patterns

```scala
Some(scala.Tuple2(1, 2)).map(((x$1) => x$1 match {
  case scala.Tuple2((x @ _), _) => x
}))
```

^ This has a few practical differences:

^ Without the flag, non-matching values will be filtered out
^ With the flag, partial patterns will trigger a warning and scala.MatchError could be thrown at runtime
^ Without the flag, you can't have patterns on the left hand side unless the right hand side has withFilter
^ With the flag, desugaring generators in for-comprehension only needs flatMap and map; withFilter is used only for desugaring if.

---

# Nifties
```scala
def fib(n: Int) = {
  def fib'(n: Int, next: Int, £: Int): Int =
    n match {
      case 0 => £
      case _ => fib'(n - 1, next + £, next)
    }
  fib'(n, 1, 0)
}

val £': Byte  = 127z
```

![right](https://cdn.eyeem.com/thumb/900/900/10765bfb0c32a665f78ad5d90cebbb0f20672917?highRes=true)

^ Mention some nifty things added to the fork

---

# [fit] Vision
:syringe: :mount_fuji:

![](https://cdn.eyeem.com/thumb/900/900/858bc4fabdc72956e045aadf29e0b58fa5162909?highRes=true)

^ no releases of 2.10
^ will soon drop 2.11, 2.12 only
^ once dotty bootstraps, will switch to it (dotty is the Third System Effect)
^ point is being on the bleeding edge
^ as you can see, we're mostly adding small things and battle-testing them
^ no one one of us is a compiler engineer
^ eventually we plan to get better at compiler development, and work on bigger, more impactful features
^ not to split the community
^ compiler plugin?

---

# [fit] Low-hanging fruits

* converting partest tests to junit
* documentation
* Reporting bugs
* Fixing bugs
* Backporting changes from typesafe scala

![right](https://cdn.eyeem.com/thumb/900/900/1e1b4616c6ba643147ff572c9592d981557470bf?highRes=true)

---

# [fit] Sci-fi future

![](https://i.imgur.com/Ei14dfw.png)

^ Don't over-promise
^ Note: we aren't compiler devs yet, we have full time jobs, and really don't know what we're doing. So don't take anything here as a promise or a final dicision or anything that has any level of commitement. The whole point of this talk is to inspire collaboration.
^ This is just an exploration of possible ideas, no commitments, etc.

---

# [fit] Refinement types

^ can base them on top of literal-based singleton types
^ going to an external SMT like z3 to prove things about types
^ talked about this last year at scala.exchange
^ generally relying on something external to figure out types would allow us to get the benifits of external research for free as well

![](http://drorbn.net/bbs/shots/Itai-111108-131013.jpg)

---

# [fit] Experiment more with stdlib

![](https://camo.githubusercontent.com/c7a1d594954b34a8277bce52343fe731b14870ad/687474703a2f2f706c61737469632d69646f6c617472792e636f6d2f6572696b2f63617473322e706e67)
![](https://camo.githubusercontent.com/c7a1d594954b34a8277bce52343fe731b14870ad/687474703a2f2f706c61737469632d69646f6c617472792e636f6d2f6572696b2f63617473322e706e67)

---

# [fit] Integrating alternative repl

![](http://lihaoyi.github.io/Ammonite/GettingStarted.png)

---

# Rethinking the role of implicits[^3]

![left](http://orig03.deviantart.net/870c/f/2013/141/2/6/mr__spock__zachary_quinto__by_lei_feiyang-d65xp7b.jpg)

[^3]: https://github.com/typelevel/scala/issues/28 -- `@implicitWeight`

---

# [fit] Hands-on

```scala
scalaVersion      := "2.11.7"
scalaOrganization := "org.typelevel"
```

^ compiled shapeless and cats with this
^ also works with scala.js
^ who has a project that's 2.11 only?

---

# [fit] Hands-on

```sh
∙ sbt -Dsbt.boot.properties=sbt.boot.properties
```

```
[repositories]
  maven-central
```

^ jcenter has a version from mid August, which has a bug
^ ONCE

---

# [fit] What do I do once I check out the repo?

![](https://cdn.eyeem.com/thumb/900/900/b3e399eb305e00d673d8db4d2a7e4a9506dbb140?highRes=true)

---
# `sbt build` vs `ant build`

^ Compile from scratch ~5mins
^ use the sbt build, it can do most of the things
^ if you're hardcore, before commiting do `ant test` (will fail right now though)
^ you can `sbt publish-local`

![](https://cdn.eyeem.com/thumb/900/900/b3e399eb305e00d673d8db4d2a7e4a9506dbb140?highRes=true)

---
# `sbt test` vs `ant test`
# `./tools/partest-ack`

^ `ant test` ~1-2 hours
^ `sbt test` ~3 minutes

![](https://cdn.eyeem.com/thumb/900/900/b3e399eb305e00d673d8db4d2a7e4a9506dbb140?highRes=true)

---

# `sbt publish` vs `ant publish-local-opt`

![](https://cdn.eyeem.com/thumb/900/900/b3e399eb305e00d673d8db4d2a7e4a9506dbb140?highRes=true)

---

# If all the things have failed, `ant all.clean`

![](https://cdn.eyeem.com/thumb/900/900/b3e399eb305e00d673d8db4d2a7e4a9506dbb140?highRes=true)

---

# [fit] To summarize

^ Beneficial for typesafe and typelevel
^ No community splitting
^ Bleeding edge
^ Try now, contribute
^ We'll be reviving the project, marking low-hanging fruit tasks, etc.

![](https://cdn.eyeem.com/thumb/900/900/df7222e32cfeba37071041878bf50cb62e709b06?highRes=true)

---

# [fit] Danke!

![](https://cdn.eyeem.com/thumb/900/900/cf7f3b70599927b329a4e56040f11e92084a0b40?highRes=true)
