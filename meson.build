project('dxvk', ['c', 'cpp'], version : 'v1.11.0', meson_version : '>= 0.58', default_options : [ 'cpp_std=c++17', 'b_vscrt=static_from_buildtype' ])

# No checks, just direct paths
dxvk_include_path = include_directories('./include', './include/vulkan', './include/spirv')

cpp = meson.get_compiler('cpp')
cc = meson.get_compiler('c')

compiler_args = ['-msse', '-msse2', '-mfpmath=sse', '-Wno-cast-function-type', '-DNOMINMAX']
link_args = ['-static', '-static-libgcc', '-static-libstdc++', '-Wl,--file-alignment=4096']

add_project_arguments(cpp.get_supported_arguments(compiler_args), language: 'cpp')
add_project_arguments(cc.get_supported_arguments(compiler_args), language: 'c')
add_project_link_arguments(cpp.get_supported_link_arguments(link_args), language: 'cpp')
add_project_link_arguments(cc.get_supported_link_arguments(link_args), language: 'c')

glsl_compiler = find_program('glslang', 'glslangValidator')
glsl_generator = generator(glsl_compiler, output: ['@BASENAME@.h'], arguments: ['-V', '--quiet', '--vn', '@BASENAME@', '@INPUT@', '-o', '@OUTPUT@'])

dxvk_version = vcs_tag(command: ['git', 'describe', '--dirty=+'], input: 'version.h.in', output: 'version.h')

subdir('src')
