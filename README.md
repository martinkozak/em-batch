EventMachine Batch
==================

**em-batch** provides ability to run more callbacked methods
of single or more objects by elegant, readable and transparent way
in a linear sequence, so subsequently in single batch.

Two classes are available, *sequencer* and more general *batch*.
Batch supports more objects in one batch, sequencer is syntactic
sugar in fact for single object.

It's implementation of the [callback-batch][9] gem, but customized
for running in [EventMachine][8] environment, so calls will be fully
multiplexed to more subsequent ticks.

See some trivial examples:

```ruby
require "eventmachine"
require "callback-batch"

class Foo
    def foo1
        yield :foo1
    end
    def foo2
        yield :foo2
    end
end

class Bar
    def bar1
        yield :bar1
    end
    def bar2
        yield :bar2
    end
end


EM::run do

    ### Sequencer

    s = CallbackSequencer::new(Foo::new)
    s.foo1
    s.foo2

    s.execute do      # now will be both methods executed
        p s.results   # will contain [:foo1, :foo2]
    end

    ### Batch

    s = CallbackBatch::new
    f = Foo::new
    b = Bar::new

    s.take(f).foo1
    s.take(b).bar2

    s.execute do      # now will be both methods executed
        p s.results   # will contain [:foo1, :bar2]
    end

end
```

Copyright
---------

Copyright &copy; 2011 &ndash; 2015 [Martin Poljak][10]. See `LICENSE.txt` for further details.

[8]: http://rubyeventmachine.com/
[9]: http://github.com/martinkozak/callback-batch/issues
[10]: http://www.martinpoljak.net/
