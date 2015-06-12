GC_CLASS
========

Metaclass providing basic object garbage collection using variable traces

Usage
-----

~~~tcl
gc_class create Foo {
	variable thing

	constructor {a_thing} {
		set thing	$a_thing
	}

	method greet {} {
		puts "hello, $thing"
	}
}

proc bar {
	Foo instvar foo1 "first example"
	$foo1 greet		;# says "hello, first example"

	Foo instvar foo2 "second example"
	$foo2 greet		;# says "hello, second example"

	Foo instvar foo2 "third example"	;# second example dies here
	$foo2 greet		;# says "hello, third example"

	Foo instvar foo4 "fourth example"
	$foo4 greet		;# says "hello, fourth example"

	set foo2	"something else"	;# third example dies here

	unset foo4		;# fourth example dies here

	Foo instvar ::foo5 "fifth example"

	# first example dies when the proc returns here (since $foo1 goes out of scope)
}

bar

# One Foo instance remains, the "fifth example"

$foo5 greet		;# says "hello, fifth example"
unset foo5		;# fifth example dies here

# No instances of Foo remain
~~~

That's about it really.  Everything else should work the same as objects created directly from oo::class.

License
-------

This package is licensed under the same terms as the Tcl core.

