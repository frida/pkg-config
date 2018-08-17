project('pkg-config', 'c',
  version : '0.29.2',
  license : 'gpl'
)

cc = meson.get_compiler('c')

glib_dep = dependency('glib-2.0')

ndebug = get_option('b_ndebug')
if ndebug == 'true' or (ndebug == 'if-release' and not get_option('debug'))
  add_project_arguments([
      '-DG_DISABLE_CAST_CHECKS',
      '-DG_DISABLE_ASSERT',
      '-DG_DISABLE_CHECKS'
    ],
    language : 'c'
  )
endif

cdata = configuration_data()
cdata.set_quoted('VERSION', meson.project_version())
cdata.set_quoted('PKG_CONFIG_SYSTEM_INCLUDE_PATH', '')
cdata.set_quoted('PKG_CONFIG_SYSTEM_LIBRARY_PATH', '')
cdata.set_quoted('PKG_CONFIG_PC_PATH', '')
cdata.set('ENABLE_DEFINE_PREFIX', 'TRUE')
cdata.set('ENABLE_INDIRECT_DEPS', 'TRUE')

headers = [
  'malloc.h',
  'sys/wait.h',
  'unistd.h',
]
foreach h : headers
  if cc.has_header(h)
    cdata.set('HAVE_' + h.underscorify().to_upper(), 1)
  endif
endforeach

configure_file(output : 'config.h', configuration : cdata)
add_project_arguments('-DHAVE_CONFIG_H', language : 'c')

sources = [
  'pkg.c',
  'parse.c',
  'rpmvercmp.c',
  'main.c'
]

executable('pkg-config', sources,
  dependencies : [glib_dep],
  install : true
)

m4data = configuration_data()
m4data.set('VERSION', meson.project_version())
configure_file(
  input : 'pkg.m4.in',
  output : 'pkg.m4',
  configuration : m4data,
  install_dir : join_paths(get_option('datadir'), 'aclocal')
)
