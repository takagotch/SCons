### scons
---
https://www.scons.org/

https://github.com/SCons/scons

```py
def add_file_to_emitter(env, emitter_name, file):
  try:
    original_emitter = env[emitter_name]
    if type(original_emitter) == list:
      original_emitter = original_emitter[0]
  except KeyError:
    original_emitter = None
  def emitter(target, source, env):
    if original_emitter:
      target, source = original_emitter(target, source, env)
    return target, source + [file]
  env[emitter_name] = emitter

env = Environment()
conf = env.CheckLib('somelib'):
  add_file_to_emitter(env, 'PROGMITTER', File('somelibreplacement.c'))
conf.Finish()

def my_emitter(target, source, env):
  '''
  '''
  if os.path.exists(source[0].path):
    return (target, source)
  altSrcName = source[0].abspath.replace(env['BUILD_DIR'] + os.sep, '')
  if os.path.exists(altSrcName):
    return (target, altSrcName)
  altSrcName = "%s/%s%s" % (File('#'), env['OTHER_BUILD_DIR'],
    source[0].path.replace(env['OTHER_BUILD_DIR'], ''))
  return (target, altSrcName)
from SCons.Tool import createObjBuilders
static_obj, shared_obj = createObjBuilders(env)
static_obj.add_emitter('.cpp', my_emitter)


```

```
```

```
```


