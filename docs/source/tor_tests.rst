.. _tor_tests:

Writing ``tor`` tests
=====================

Documentation from [torTests]_ (Jun 2018)


Writing tests
---------------

Modify the corresponding ``src/test/test_foo.c``

If creating a new function, add it to the ``struct testcase_t foo_tests[]``
The function must have::
    
     done: <foo>;

If creating a new file in ``src/test``:
- add the file in ``src/test/include.ac``
- add ``extern struct testcase_t foo_tests[];`` in  ``src/test/test.h``


Testing result function
-------------------------
::

    tt_int_op(0, OP_EQ, foo_fn());

Testing logs
--------------
::

    setup_full_capture_of_logs(LOG_DEBUG);

    expect_single_log_msg_containing("foo text");

Mocking
~~~~~~~~

In ``src/or/foo.h``::

    static long static_var = 0;
    MOCK_DECL(long,foo_fn,(void));

In ``src/or/foo.c``::

    MOCK_IMPL(long,
    foo_fn,(void))
    {
      return static_var;
    }

In ``src/test/test_foo.c``::

    #include "foo.h"
    
    static long mock_static_var = x;

    static long
    mock_foo_fn(void)
    {
      mock_static_var = y;
      return mock_static_var;
    }
    

Include ``SRCDIR`` in ``src/test/configure.ac`` to be able to open data file from test::

    if test "x$SRCDIR" = "x"; then
      SRCDIR=`pwd`
    fi
    AH_TEMPLATE([SRCDIR],[tor's sourcedir directory])
    AC_DEFINE_UNQUOTED(SRCDIR,"$SRCDIR")

Running tests
--------------

All::

    make check

A single test::

    src/test/test --debug bar/foo


Before committing
------------------

::

    ./config --enable-fragile-hardening

    make check-local
    make check-spaces
    make check-changes
    make check-memleaks