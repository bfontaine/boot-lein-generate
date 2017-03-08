# boot-lein-generate

[![Clojars Project](https://img.shields.io/clojars/v/sparkfund/boot-lein-generate.svg)](https://clojars.org/sparkfund/boot-lein-generate)

[Boot](https://github.com/boot-clj/boot) task for generating a `project.clj` from your Boot project.
This will allow [Cursive IDE](https://cursive-ide.com/) to work with your Boot project.

```clojure
;in your build.boot
(set-env! :dependencies '[[sparkfund/boot-lein-generate "0.2.0"]])
(require '[boot.lein :refer :all])

;now you can run `boot write-project-clj`
```

Forked from [onetom's repo](https://github.com/onetom/boot-lein-generate) (which was itself forked
from [darongmean's repo](https://github.com/darongmean/boot-lein-generate)) to add some extra
properties and configurability in scenarios where `boot.core/get-env` doesn't include all the
information you need it to include in the `project.clj`.  As an example, you might wish to add
`:repl-options` into `project.clj`, or extend the set of directories Cursive will tell IntelliJ to
mark as source roots.


## Usage

Add it to your `build.boot` (see above) and then run `boot write-project.clj`.

If there are additional properties you want to set in the generated `project.clj`, or if you'd like to replace the
values generated by this task with different values, you can either put a map into a Boot environment property
named `:boot.lein/project-clj`, or pass the map as the `override` option to the `write-project-clj` task:

```clojure
;first option: put the map into the Boot environment
;note that values passed as an option to the task will override values from this map
(set-env! :boot.lein/project-clj {:url "https://github.com/SparkFund/boot-lein-generate"})

;second option: pass it as an option to the task
(task-options! write-project-clj {:override {:url "https://github.com/SparkFund/boot-lein-generate"}})

;or invoke the task directly from some other task
(deftask whatever
  [] ;...
  (write-project-clj {:override {:url "https://github.com/SparkFund/boot-lein-generate"}}))
```

You can also specify these overrides from the command line, although you have to be a little
careful because the values you provide will be interpreted as code.  Here's a sample showing a few
different types of options:

```sh
$ boot write-project-clj -o ':url="http://example.com"' -o ':offline?=true' -o ':pedantic?=:abort'
```

This is equivalent to setting the task options like this:

```clojure
(task-options! write-project-clj {:override {:url "http://example.com", :offline? true, :pedantic? :abort}})
```

## License

Copyright 2016-2017
[Darong Mean](https://github.com/darongmean/boot-lein-generate),
[Tamas Herman](https://github.com/onetom/boot-lein-generate),
and [SparkFund](https://github.com/SparkFund/boot-lein-generate)

Distributed under the [Eclipse Public License, Version 1.0](LICENSE.md), the same as Clojure.
