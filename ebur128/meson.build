cc = meson.get_compiler('c')
has_queue = cc.compiles('#include <sys/queue.h>\nLIST_HEAD(listhead, entry) head;\nint main() { return 0; }')
src_shared = files(['ebur128.c'])
inc = include_directories('.')
m_dep = cc.find_library('m', required: false)

if not has_queue and not get_option('ENABLE_INTERNAL_QUEUE_H')
    error('System queue does not exit and configuration does not specify use of internal queue implementation.')
endif

if get_option('ENABLE_INTERNAL_QUEUE_H')
    queue_dep = declare_dependency(include_directories: include_directories('queue'))
else
    queue_dep = dependency()
endif

if cc.get_id() == 'msvc'
    add_project_arguments('-D_USE_MATH_DEFINES', '/arch:SSE2', '-D __SSE2_MATH__')
endif

# if cc.get_id() == 'msvc'
#     src_shared += files(['ebur128.def'])
# endif
ebur128_lib = library('ebur128', src_shared, dependencies: [m_dep, queue_dep], install: true)
install_headers(files('ebur128.h'), subdir: 'ebur128')

ebur128_dep = declare_dependency(include_directories: inc, link_with: ebur128_lib)

pkgconfig_data = configuration_data()
pkgconfig_data.set('CMAKE_INSTALL_PREFIX', get_option('prefix'))
pkgconfig_data.set('CMAKE_INSTALL_LIBDIR', get_option('libdir'))
pkgconfig_data.set('EBUR128_VERSION', meson.version())
pkgconfig_data.set('includedir', get_option('includedir'))
pkgconfig_data.set('prefix', get_option('prefix'))
pkgconfig_data.set('libdir', get_option('libdir'))
configure_file(input: 'libebur128.pc.cmake', output: 'libebur128.pc', configuration: pkgconfig_data, install: true, install_dir: get_option('libdir') / 'pkgconfig')
# pkg = import('pkgconfig')
# pkg.generate(libraries: ebur128_lib,
#     subdirs: inc,
#     version: meson.version(),
#     name: 'libebur128',
#     filebase: 'ebur128',
#     description: 'EBU R128 standard for loudness normalization'
# )
