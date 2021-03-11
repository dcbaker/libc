# SPDX-license-identifier: Apatche-2.0
# Copyright © 2021 Intel Corporation

project(
  'libc',
  'rust',
  version : '0.2.88',
  license : 'Apache-2.0 OR MIT',
  default_options : ['default_library=static'],
)

options = []
if host_machine.system() == 'freebsd'
  ver = run_command(find_program('uname'), '-r').stdout()
  # TODO: rustc_dep_of_std
  if ver.startswith('10')
    options += 'freebsd11'
  elif ver.startswith('11')
    options += 'freebsd11'
  elif ver.startswith('12')
    options += 'freebsd12'
  elif ver.startswith('13')
    options += 'freebsd13'
  else
    options += 'freebsd11'
  endif
endif

rust = meson.get_compiler('rust')

# TODO: these probably need feature options
if rust.version().version_compare('>= 1.15')  # TODO: rustc_dep_of_std
  options += ['libc_priv_mod_use']
endif
if rust.version().version_compare('>= 1.19')  # TODO: rustc_dep_of_std
  options += ['libc_union']
endif
if rust.version().version_compare('>= 1.24')  # TODO: rustc_dep_of_std
  options += ['libc_const_size_of']
endif
if rust.version().version_compare('>= 1.25')  # TODO: rustc_dep_of_std
  options += ['libc_align']
endif
if rust.version().version_compare('>= 1.30')  # TODO: rustc_dep_of_std
  options += ['libc_core_cvoid']
endif
if rust.version().version_compare('>= 1.33')  # TODO: rustc_dep_of_std
  options += ['libc_packagedN', 'libc_cfg_target_vendor']
endif

libs = []
if host_machine.system() == 'darwin'
  libs += rust.find_library('iconv')
endif

libc_args = []
foreach o : options
  libc_args += ['--cfg', o]
endforeach

library(
  'libc',
  'src/lib.rs',
  rust_args : libc_args,
  link_with : libs,
)