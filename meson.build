project('dxvk', ['c', 'cpp'], version : 'v1.11.0', meson_version : '>= 0.58', default_options : [ 'cpp_std=c++17', 'b_vscrt=static_from_buildtype' ])

cpu_family = target_machine.cpu_family()
platform   = target_machine.system()

cpp = meson.get_compiler('cpp')
cc = meson.get_compiler('c')
dxvk_is_msvc = cpp.get_id() == 'msvc'

compiler_args = ['-msse', '-msse2', '-mfpmath=sse', '-Wno-cast-function-type']
link_args = []

# This tells the compiler to look inside your include folder for everything
dxvk_include_path = include_directories('./include')

if platform == 'windows'
  compiler_args += ['-DNOMINMAX', '-D_WIN32_WINNT=0xa00']
  if not dxvk_is_msvc
    link_args += ['-static', '-static-libgcc', '-static-libstdc++', '-Wl,--file-alignment=4096']
  endif
  lib_d3d9 = cpp.find_library('d3d9')
  lib_d3d11 = cpp.find_library('d3d11')
  lib_dxgi = cpp.find_library('dxgi')
  dxvk_wsi = 'win32'
endif

add_project_arguments(cpp.get_supported_arguments(compiler_args), language: 'cpp')
add_project_arguments(cc.get_supported_arguments(compiler_args), language: 'c')

glsl_compiler = find_program('glslang', 'glslangValidator')
glsl_generator = generator(glsl_compiler, output: ['@BASENAME@.h'], arguments: ['-V', '--quiet', '--vn', '@BASENAME@', '@INPUT@', '-o', '@OUTPUT@'])

dxvk_version = vcs_tag(command: ['git', 'describe', '--dirty=+'], input: 'version.h.in', output: 'version.h')

subdir('src')
