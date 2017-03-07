include_defs('//BUCKAROO_DEPS')

def merge_dicts(x, y):
  z = x.copy()
  z.update(y)
  return z

# TODO: Allow platform injection
# Currently we just use the supplied scripts to 
# generate any platform information. 
# Instead, we should add these as parameters 
# to this recipe. 
genrule(
  name = 'configure',
  out = 'out',
  srcs = glob([
    'Include/**/*.h',
    'Objects/**/*.c',
    'Modules/**/*.c',
    'Modules/**/*.in',
    'Modules/**/*.dist',
    'Modules/makesetup',
    'Misc/**/*.in',
    'config.guess',
    'config.sub',
    'configure',
    'configure.ac',
    'pyconfig.h.in',
    'Makefile.pre.in',
    'install-sh',
  ]),
  cmd = 'cp -r $SRCDIR $OUT && cd $OUT && ./configure ',
)

genrule(
  name = 'pyconfig.h',
  out = 'pyconfig.h',
  cmd = 'cp $(location :configure)/pyconfig.h $OUT',
)

cxx_library(
  name = 'cpython',
  header_namespace = '',
  exported_headers = merge_dicts(subdir_glob([
    ('Include', '**/*.h'),
  ]), {
    'pyconfig.h': ':pyconfig.h',
  }),
  srcs = glob([
    'Objects/**/*.c',
  ]),
  visibility = [
    'PUBLIC',
  ],
  deps = BUCKAROO_DEPS,
)
