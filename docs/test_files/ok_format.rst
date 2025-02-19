Python Format
=================

Otter has different test file formats depending on which language you are grading. Python test files 
follow the OK format, a legacy of the OkPy autograder that Otter inherits from. R test files are 
more like unit tests and rely on the testthat library to run tests with expectations.

Otter requires OK-formatted tests to check students' work against. These have a very specific 
format, described in detail in the `OkPy documentation 
<https://okpy.github.io/documentation/client.html#ok-client-setup-ok-tests>`_. There is also a 
resource we developed on writing autograder tests that can be found `here 
<https://autograder-tests.rtfd.io>`_; this guide details things like the doctest format, the 
pitfalls of string comparison, and seeding tests.


.. _test_files_ok_format_caveats:

Caveats
-------

While Otter uses OK format, there are a few caveats to the tests when using them with Otter.

* Otter only allows a single suite in each test, although the suite can have any number of cases. 
  This means that ``test["suites"]`` should be a ``list`` of length 1, whose only element is a 
  ``dict``.
* Otter uses the ``"hidden"`` key of each test case only on Gradescope. When displaying results on 
  Gradescope, the ``test["suites"][0]["cases"][<int>]["hidden"]`` should evaluate to a boolean that 
  indicates whether or not the test is hidden. The behavior of showing and hiding tests is described 
  in :ref:`workflow_executing_submissions_gradescope`.


Writing OK Tests
----------------

We recommend that you develop assignments using :ref:`Otter Assign <otter_assign>`, a tool 
which will generate these test files for you. If you already have assignments or would prefer to 
write them yourself, you can find an online `OK test generator <https://oktests.chrispyles.io>`_ 
that will assist you in generating these test files without using Otter Assign.


Sample Test
-----------

Here is an annotated sample OK test:

.. code-block:: python

    test = {
        "name": "q1",       # name of the test
        "points": 1,        # number of points for the entire suite
        "suites": [         # list of suites, only 1 suite allowed!
            {
                "cases": [                  # list of test cases
                    {                       # each case is a dict
                        "code": r"""        # test, formatted for Python interpreter
                        >>> 1 == 1          # note that in any subsequence line of a multiline
                        True                # statement, the prompt becomes ... (see below)
                        """,
                        "hidden": False,    # used to determine case visibility on Gradescope
                        "locked": False,    # ignored by Otter
                    }, 
                    {
                        "code": r"""
                        >>> for i in range(4):
                        ...     print(i == 1)
                        False
                        True
                        False
                        False
                        """,
                        "hidden": False,
                        "locked": False,
                    }, 
                ],
                "scored": False,            # ignored by Otter
                "setup": "",                # ignored by Otter
                "teardown": "",             # ignored by Otter
                "type": "doctest"           # the type of test; only "doctest" allowed
            },
        ]
    }
