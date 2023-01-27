# Staggered start of ctest tests with cmake

For certain types of cmake/ctest tests it can be handy to offset their startup
to prevent simultaneuous access to common test resources. This can either
improve test stability or test speed if tests are executed in parallel.

This repository contains a example configuration that runs on Linux which 
utilizes ctest resource locks and fixtures to stagger the startup time of tests
with a small delay.

May it be helpful to someone who searches for a solution to the same problem!

# How to use?

Create a build directory and configure the project using:

~~~~~~
cmake [SOURCE-PATH]
~~~~~~

After configuration, 20 tests will be created:

~~~~~~
Test project /my/build-dir
  Test  #1: test-1
  Test  #2: test-2
  Test  #3: test-3
  Test  #4: test-4
  Test  #5: test-5
  Test  #6: test-6
  Test  #7: test-7
  Test  #8: test-8
  Test  #9: test-9
  Test #10: test-10
  Test #11: stagger:test-1
  Test #12: stagger:test-2
  Test #13: stagger:test-3
  Test #14: stagger:test-4
  Test #15: stagger:test-5
  Test #16: stagger:test-6
  Test #17: stagger:test-7
  Test #18: stagger:test-8
  Test #19: stagger:test-9
  Test #20: stagger:test-10

Total Tests: 20
~~~~~~

The stagger-tests are "artificial tests" which introduce a delay before
other test so that those are run in parallel but their startup is delayed
by a small delay.

Run the tests with:

~~~~~~
ctest -j10
~~~~~~

# Limitations

- The stagger tests are annoying visual noise in the test reports.
