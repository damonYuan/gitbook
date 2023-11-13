python metaclass
====

In backtrader `py3.py` class, there is a magic method which is used extensively by the code.

```
# This is from Armin Ronacher from Flash simplified later by six
def with_metaclass(meta, *bases):
    """Create a base class with a metaclass."""
    # This requires a bit of explanation: the basic idea is to make a dummy
    # metaclass for one level of class instantiation that replaces itself with
    # the actual metaclass.
    class metaclass(meta):
        def __new__(cls, name, this_bases, d):
            return meta(name, bases, d)
    return type.__new__(metaclass, str('temporary_class'), (), {})
```

The `with_metaclass()` function makes use of the fact that metaclasses are 

- a) inherited by subclasses, and 
- b) a metaclass can be used to generate new classes and 
- c) when you subclass from a base class with a metaclass, creating the actual subclass object is delegated to the metaclass.
- d) `type.__new__(metaclass, 'temporary_class', (), {})` uses the `metaclass` metaclass to create a new class object named `temporary_class` that is entirely empty otherwise. `type.__new__(metaclass, ...)` is used instead of `metaclass(...)` to avoid using the special `metaclass.__new__()` implementation that is needed for the slight of hand in a next step to work.

    If `metaclass(...)` is used there will be an extra `temporary_class` base class.
    ```
    # print(Foo.__mro__)
    (<class 'metaclass_tests.Foo'>, <class 'metaclass_tests.temporary_class'>, <class 'object'>)
    ```

It effectively creates a new, temporary base class with a temporary metaclass `metaclass` that, when used to create the subclass swaps out the temporary base class.

and it could be used as 

```
class Meta(type):
    def __new__(cls, clsname, bases, attrs):
        print("Meta __new__")
        return super(Meta, cls).__new__(cls, clsname, bases, attrs)

    def __init__(cls, name, bases, attrs):
        print("Meta __init__")
        type.__init__(cls, name, bases, attrs)

    def __call__(cls, *args, **kwargs):
        print("Meta __call__")
        return super(Meta, cls).__call__(*args, **kwargs)


temporary_class = with_metaclass(Meta, object) # L1


class Foo(temporary_class): # L2
    def __new__(cls, *args, **kwargs):
        print("Rectangle __new__")
        return super(Foo, cls).__new__(cls, *args, **kwargs)

    def __int__(self, *args, **kwargs):
        print("Foo __int__")
```

1. At `L1` a class named `temporary_class` has been created from `metaclass` and assigned to a local variable also named `temporary_class`.

2. At `L2` when the `temporary_class` is used as the base class, creating the actual `Foo` class will be delegated to the temporary_class

3. the `def __new__(cls, name, this_bases, d)` function will be called and `metaclass`, `Foo`, `temporary_class` and `{}` will be passed in. Then `meta(name, bases, d)` will be used to create the actual `Foo` class. Note that as no instance of `metaclass` is return in the `__new__` method, the `__init__` method inside the `metaclass` will never be called.

# Tests

```
import unittest


def with_metaclass_old(meta, *bases):
    class metaclass(meta):
        def __new__(cls, name, this_bases, d):
            print("metaclass __new__")
            return meta(name, bases, d)

    print("with_metaclass")
    return metaclass(str('temporary_class'), (), {})


def with_metaclass(meta, *bases):
    class metaclass(meta):
        def __new__(cls, name, this_bases, d):
            print("metaclass __new__")
            # return super(meta, cls).__new__(cls, name, bases, d) # metaclass.__init__ being called
            # return type.__new__(cls, name, bases, d) # metaclass.__init__ being called
            # return meta.__new__(cls, name, bases, d) # metaclass.__init__ being called
            return meta(name, bases, d)

        def __init__(cls, name, bases, attrs):
            print("metaclass __init__")

    print("with_metaclass")
    return type.__new__(metaclass, str('temporary_class'), (), {})


class Meta(type):
    def __new__(cls, clsname, bases, attrs):
        print("Meta __new__")
        return super(Meta, cls).__new__(cls, clsname, bases, attrs)

    def __init__(cls, name, bases, attrs):
        print("Meta __init__")
        type.__init__(cls, name, bases, attrs)

    def __call__(cls, *args, **kwargs):
        print("Meta __call__")
        return super(Meta, cls).__call__(*args, **kwargs)


temporary_class = with_metaclass(Meta, object)


class Foo(temporary_class):
    def __new__(cls, *args, **kwargs):
        print("Foo __new__")
        return super(Foo, cls).__new__(cls, *args, **kwargs)

    def __int__(self, *args, **kwargs):
        print("Rectangle __int__")


class MetaclassTests(unittest.TestCase):
    def setUp(self):
        print(">>>> classes have been setup")
        print(type(Foo))
        print(Foo.__mro__)

    def test_create_instance(self):
        print(">>>> test_create_instance")
        foo = Foo()


    def tearDown(self):
        pass


if __name__ == "__main__":
    unittest.main()

```

# Reference
1. [Method Resolution Order (MRO)](https://stackoverflow.com/questions/3277367/how-does-pythons-super-work-with-multiple-inheritance)
2. [Creating a singleton in Python](https://stackoverflow.com/questions/6760685/creating-a-singleton-in-python)
3. [What are metaclasses in Python?](https://stackoverflow.com/questions/100003/what-are-metaclasses-in-python)
4. [Supercharge Your Classes With Python super()](https://realpython.com/python-super/) and mixin
5. [Python type() Function](https://www.digitalocean.com/community/tutorials/python-type)
6. [What is the difference between type and type.__new__ in python?](https://stackoverflow.com/questions/2608708/what-is-the-difference-between-type-and-type-new-in-python)
7. [Python Metaclass : Understanding the 'with_metaclass()'](https://stackoverflow.com/questions/18513821/python-metaclass-understanding-the-with-metaclass)
8. [Cross-Python metaclasses](https://www.zopatista.com/python/2014/03/14/cross-python-metaclasses/)