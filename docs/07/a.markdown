Planning for Any-thing
----------------------

Thanks to one of the darker corners of the Scala type system
(*variance*), it's a cinch to define intents that work with all plans.

### Agnostic Intents

```scala
import unfiltered.request._
import unfiltered.response._

object Hello {
  val intent = unfiltered.Cycle.Intent[Any, Any] {
    case _ => ResponseString("Hello")
  }
}
```

The object `Hello` defines an intent with underlying request
and response types of `Any`. As a result, the intent can not
statically expect a particular underlying request or response
binding. This makes sense, as we want to make an intent that
works with any of them.

### Specific Plans

The next step is to supply the generic intent to different plans.

```scala
object HelloFilter extends
       unfiltered.filter.Planify(Hello.intent)

object HelloHandler extends
       unfiltered.netty.cycle.Planify(Hello.intent)
```

As usual the plans are actual servlet filters or Netty handlers, so
you could use them with a server you have configured separately. Or,
you can configure and start your server in a console with Unfiltered.

### Servers

You could run these in a `main()` method, or on the Scala console.

```scala
// runs until you press any key
unfiltered.jetty.Http.anylocal.plan(HelloFilter).run()

// runs until you press any key
unfiltered.netty.Http.anylocal.plan(HelloHandler).run()
```
