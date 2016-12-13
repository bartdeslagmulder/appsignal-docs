---
title: "Ignore instrumentation"
---

Sometimes you have very large background jobs that generate thousands of
repeated queries over and over again. In this case not all the information the
instrumentation records is as important as elsewhere in the application.

To filter out some of these instrumentation events from being send to AppSignal
you can tell the Ruby gem to stop instrumenting a block of code.

```ruby
class BackgroundWorker
  def perform
    site = Site.find(1)
    10_000.times do
      Appsignal.without_instrumentation do
        site.perform_many_queries
      end
    end
  end
end
```

The code above will only instrument the `Site.find(1)` query and ignore all the
queries generated by `perform_many_queries`.