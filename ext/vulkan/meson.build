if get_option('vulkan').disabled()
  subdir_done()
endif
glslc = find_program('glslc', required: get_option('vulkan'))
if not glslc.found()
  subdir_done()
endif

subdir('shaders')

vulkan_sources = [
  'gstvulkan.c',
  'vkcolorconvert.c',
  'vkdownload.c',
  'vkfullscreenrender.c',
  'vkimageidentity.c',
  'vksink.c',
  'vkshader.c',
  'vkupload.c',
  'vkviewconvert.c',
]

vulkan_plugin_enum_headers = [
  'vkviewconvert.h',
]

vulkan_plugin_enums = gnome.mkenums_simple('gstvulkan-plugins-enumtypes',
    sources : vulkan_plugin_enum_headers,
    body_prefix : '#ifdef HAVE_CONFIG_H\n#include "config.h"\n#endif',
    header_prefix : '#include <gst/gst.h>')
vulkan_plugin_enumtypes_c = vulkan_plugin_enums[0]
vulkan_plugin_enumtypes_h = vulkan_plugin_enums[1]
vulkan_plugin_gen_sources = [vulkan_plugin_enumtypes_h]

gstvulkan_plugin = library('gstvulkan',
  vulkan_sources, vulkan_compiled_shader_sources, vulkan_plugin_enumtypes_c, vulkan_plugin_enumtypes_h,
  c_args : gst_plugins_bad_args,
  objc_args : gst_plugins_bad_args,
  link_args : noseh_link_args,
  include_directories : [configinc],
  dependencies : [gstvideo_dep, gstbase_dep, gstvulkan_dep, vulkan_dep],
  install : true,
  install_dir : plugins_install_dir,
)
pkgconfig.generate(gstvulkan_plugin, install_dir : plugins_pkgconfig_install_dir)
plugins += [gstvulkan_plugin]
