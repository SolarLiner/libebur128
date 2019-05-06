sndfile_dep = dependency('sndfile', required: false)

if not sndfile_dep.found() or not get_option('ENABLE_TESTS')
    subdir_done()
endif

test_exe = executable('r128-test-library', files('test.c'), dependencies: [sndfile_dep, ebur128_dep])

test('ebur128 tests', test_exe)

minexample_exe = executable('ebur128-minimalexample', files('minimal-example.c'), dependencies: [sndfile_dep, ebur128_dep])
