app_sources = ['gstappsrc.c', 'gstappsink.c']

app_mkenum_headers = [
  'gstappsrc.h',
]

app_headers = app_mkenum_headers + [ 'app.h', 'gstappsink.h' ]
install_headers(app_headers, subdir : 'gstreamer-1.0/gst/app/')

app_enums = gnome.mkenums_simple('app-enumtypes',
  sources : app_mkenum_headers,
  header_prefix : '#include <gst/gst.h>',
  decorator : 'GST_EXPORT',
  install_header: true,
  install_dir : 'include/gstreamer-1.0/gst/app/')
gstapp_c = app_enums[0]
gstapp_h = app_enums[1]

app_gen_sources = [gstapp_h]

gstapp = library('gstapp-@0@'.format(api_version),
  app_sources, gstapp_h, gstapp_c,
  c_args : gst_plugins_base_args,
  include_directories: [configinc, libsinc],
  version : libversion,
  soversion : soversion,
  install : true,
  dependencies : [gst_base_dep],
  vs_module_defs: vs_module_defs_dir + 'libgstapp.def',
)

if build_gir
  gst_gir_extra_args = gir_init_section + [ '--c-include=gst/app/app.h' ]
  app_gen_sources += [gnome.generate_gir(gstapp,
    sources : app_sources + app_headers + [gstapp_c] + [gstapp_h],
    namespace : 'GstApp',
    nsversion : api_version,
    identifier_prefix : 'Gst',
    symbol_prefix : 'gst',
    export_packages : 'gstreamer-app-1.0',
    includes : ['Gst-1.0', 'GstBase-1.0'],
    install : true,
    extra_args : gst_gir_extra_args,
    dependencies : [gst_dep, gst_base_dep]
  )]
endif

app_dep = declare_dependency(link_with: gstapp,
  include_directories : [libsinc],
  dependencies : [gst_base_dep],
  sources : app_gen_sources)
