project('libcxx', 'cpp',
  default_options: [
    'cpp_std=c++26',
  ],
)

abi_srcdir = 'libcxxabi' / 'src'
abi_incdir = 'libcxxabi' / 'include'
lib_srcdir = 'libcxx' / 'src'
lib_incdir = 'libcxx' / 'include'

host_os = host_machine.system()
is_static = get_option('default_library') == 'static'

install_headers([
    abi_incdir / 'cxxabi.h',
    abi_incdir / '__cxxabi_config.h',
  ],
  subdir: 'c++',
)
install_subdir(lib_incdir,
  install_dir: get_option('includedir') / 'c++',
  strip_directory: true,
  exclude_files: [
    'CMakeLists.txt',
    '__config_site.in',
    'module.modulemap.in',
    'libcxx.imp',
    '__cxx03' / 'module.modulemap',
  ],
)

site = configuration_data()
site.set('_LIBCPP_ABI_VERSION', 1)
site.set('_LIBCPP_ABI_NAMESPACE', '__1')
site.set('_LIBCPP_ABI_FORCE_ITANIUM', 0)
site.set('_LIBCPP_ABI_FORCE_MICROSOFT', 0)
site.set('_LIBCPP_HAS_THREADS', 1)
site.set('_LIBCPP_HAS_MONOTONIC_CLOCK', 1)
site.set('_LIBCPP_HAS_MUSL_LIBC', 0)
site.set('_LIBCPP_HAS_THREAD_API_PTHREAD', 1)
site.set('_LIBCPP_HAS_THREAD_API_EXTERNAL', 0)
site.set('_LIBCPP_HAS_THREAD_API_WIN32', 0)
site.set('_LIBCPP_HAS_THREAD_API_C11', 0)
if is_static
  site.set('_LIBCPP_DISABLE_VISIBILITY_ANNOTATIONS', true)
endif
site.set('_LIBCPP_HAS_VENDOR_AVAILABILITY_ANNOTATIONS', 0)
site.set('_LIBCPP_HAS_FILESYSTEM', 1)
site.set('_LIBCPP_HAS_RANDOM_DEVICE', 1)
site.set('_LIBCPP_HAS_LOCALIZATION', 1)
site.set('_LIBCPP_HAS_UNICODE', 1)
site.set('_LIBCPP_HAS_WIDE_CHARACTERS', 1)
site.set('_LIBCPP_HAS_TIME_ZONE_DATABASE', 0)
site.set('_LIBCPP_INSTRUMENTED_WITH_ASAN', 0)
site.set('_LIBCPP_PSTL_BACKEND_STD_THREAD', true)
site.set('_LIBCPP_HARDENING_MODE_DEFAULT', 2)
site.set('_LIBCPP_ASSERTION_SEMANTIC_DEFAULT', 2)
site.set('_LIBCPP_LIBC_PICOLIBC', 0)
site.set('_LIBCPP_LIBC_NEWLIB', 0)
site.set('_LIBCPP_LIBC_LLVM_LIBC', 0)
site.set('_LIBCPP_ABI_DEFINES', '')
site.set('_LIBCPP_EXTRA_SITE_DEFINES', '')

configure_file(
  input: lib_incdir / '__config_site.in',
  output: '__config_site',
  configuration: site,
  format: 'cmake@',
  install: true,
  install_dir: get_option('includedir') / 'c++',
)
configure_file(
  input: 'libcxx' / 'vendor' / 'llvm' / 'default_assertion_handler.in',
  output: '__assertion_handler',
  copy: true,
  install: true,
  install_dir: get_option('includedir') / 'c++',
)

abi_sources = [
  abi_srcdir / 'cxa_aux_runtime.cpp',
  abi_srcdir / 'cxa_default_handlers.cpp',
  abi_srcdir / 'cxa_demangle.cpp',
  abi_srcdir / 'cxa_exception_storage.cpp',
  abi_srcdir / 'cxa_guard.cpp',
  abi_srcdir / 'cxa_handlers.cpp',
  abi_srcdir / 'cxa_vector.cpp',
  abi_srcdir / 'cxa_virtual.cpp',
  abi_srcdir / 'stdlib_exception.cpp',
  abi_srcdir / 'stdlib_stdexcept.cpp',
  abi_srcdir / 'stdlib_typeinfo.cpp',
  abi_srcdir / 'abort_message.cpp',
  abi_srcdir / 'fallback_malloc.cpp',
  abi_srcdir / 'private_typeinfo.cpp',
  abi_srcdir / 'stdlib_new_delete.cpp',
  abi_srcdir / 'cxa_exception.cpp',
  abi_srcdir / 'cxa_personality.cpp',
]
if host_os not in ['windows', 'darwin']
  abi_sources += abi_srcdir / 'cxa_thread_atexit.cpp'
endif

lib_sources = [
  lib_srcdir / 'algorithm.cpp',
  lib_srcdir / 'any.cpp',
  lib_srcdir / 'bind.cpp',
  lib_srcdir / 'call_once.cpp',
  lib_srcdir / 'charconv.cpp',
  lib_srcdir / 'chrono.cpp',
  lib_srcdir / 'error_category.cpp',
  lib_srcdir / 'exception.cpp',
  lib_srcdir / 'expected.cpp',
  lib_srcdir / 'filesystem' / 'filesystem_clock.cpp',
  lib_srcdir / 'filesystem' / 'filesystem_error.cpp',
  lib_srcdir / 'filesystem' / 'path.cpp',
  lib_srcdir / 'functional.cpp',
  lib_srcdir / 'hash.cpp',
  lib_srcdir / 'memory.cpp',
  lib_srcdir / 'memory_resource.cpp',
  lib_srcdir / 'new_handler.cpp',
  lib_srcdir / 'new_helpers.cpp',
  lib_srcdir / 'optional.cpp',
  lib_srcdir / 'print.cpp',
  lib_srcdir / 'random_shuffle.cpp',
  lib_srcdir / 'ryu' / 'd2fixed.cpp',
  lib_srcdir / 'ryu' / 'd2s.cpp',
  lib_srcdir / 'ryu' / 'f2s.cpp',
  lib_srcdir / 'stdexcept.cpp',
  lib_srcdir / 'string.cpp',
  lib_srcdir / 'system_error.cpp',
  lib_srcdir / 'typeinfo.cpp',
  lib_srcdir / 'valarray.cpp',
  lib_srcdir / 'variant.cpp',
  lib_srcdir / 'vector.cpp',
  lib_srcdir / 'verbose_abort.cpp',
  lib_srcdir / 'atomic.cpp',
  lib_srcdir / 'barrier.cpp',
  lib_srcdir / 'condition_variable_destructor.cpp',
  lib_srcdir / 'condition_variable.cpp',
  lib_srcdir / 'future.cpp',
  lib_srcdir / 'mutex_destructor.cpp',
  lib_srcdir / 'mutex.cpp',
  lib_srcdir / 'shared_mutex.cpp',
  lib_srcdir / 'thread.cpp',
  lib_srcdir / 'random.cpp',
  lib_srcdir / 'fstream.cpp',
  lib_srcdir / 'ios.cpp',
  lib_srcdir / 'ios.instantiations.cpp',
  lib_srcdir / 'iostream.cpp',
  lib_srcdir / 'locale.cpp',
  lib_srcdir / 'ostream.cpp',
  lib_srcdir / 'regex.cpp',
  lib_srcdir / 'strstream.cpp',
  lib_srcdir / 'text_encoding.cpp',
  lib_srcdir / 'filesystem' / 'directory_entry.cpp',
  lib_srcdir / 'filesystem' / 'directory_iterator.cpp',
  lib_srcdir / 'filesystem' / 'operations.cpp',
  lib_srcdir / 'filesystem' / 'int128_builtins.cpp',
]

incdirs = include_directories(
  '.',
  lib_srcdir,
  lib_incdir,
  abi_incdir,
  'llvm-libc',
)

cpp = meson.get_compiler('cpp')

cpp_args = [
  '-nostdinc++',
  '-fno-aligned-allocation',
  '-fvisibility-inlines-hidden',
  '-fvisibility=hidden',
  '-fsized-deallocation',
  '-fstrict-aliasing',
  '-D_LIBCPP_BUILDING_LIBRARY',
  '-D_LIBCPP_AVAILABILITY_MINIMUM_HEADER_VERSION=2',
  '-D_LIBCPP_REMOVE_TRANSITIVE_INCLUDES',
  '-DLIBC_NAMESPACE=__llvm_libc_common_utils',
]
if is_static
  cpp_args += [
    '-D_LIBCPP_DISABLE_VISIBILITY_ANNOTATIONS=',
    cpp.get_supported_arguments('-fvisibility-global-new-delete=force-hidden'),
  ]
endif

libdir = get_option('libdir') / 'c++'

libcxxabi = library('c++abi', abi_sources,
  cpp_args: cpp_args + [
    '-D_LIBCXXABI_BUILDING_LIBRARY',
    '-DLIBCXXABI_SILENT_TERMINATE',
  ],
  include_directories: incdirs,
  install: true,
  install_dir: libdir,
)

libcxx = library('c++', lib_sources,
  cpp_args: cpp_args + [
    '-DLIBCXX_BUILDING_LIBCXXABI',
  ],
  include_directories: incdirs,
  link_with: libcxxabi,
  install: true,
  install_dir: libdir,
)
