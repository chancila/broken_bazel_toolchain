this repository illustrates a bug in bazel

building the target via

```
bazelisk run //:test_bin
```

causes a header inclusion error:

```
(cd /Users/chancila/Snapchat/Dev/.cache/_bazel_root/c3aade7b20f94326e4f8d77b068ab2d6/execroot/__main__ && \
  exec env - \
    PATH=/Users/chancila/google-cloud-sdk/bin:/Users/chancila/.pyenv/shims:/Users/chancila/.rbenv/shims:/Users/chancila/.cargo/bin:/Users/chancila/bin:/Users/chancila/go/bin:/Users/chancila/.cargo/bin:/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin:/usr/local/munki:/opt/X11/bin:/Library/Apple/usr/bin:/usr/local/opt/fzf/bin:/Users/chancila/Library/Android/sdk/platform-tools:/Users/chancila/Snapchat/Dev/flutter/bin \
    PWD=/proc/self/cwd \
  external/llvm_toolchain/bin/cc_wrapper.sh -U_FORTIFY_SOURCE -fstack-protector -fno-omit-frame-pointer -fcolor-diagnostics -Wall -Wthread-safety -Wself-assign -isystem bazel-out/host/bin/external/llvm_toolchain/ '-std=c++17' '-stdlib=libc++' -MD -MF bazel-out/darwin-fastbuild/bin/_objs/test_bin/test_bin.pic.d -fPIC -iquote . -iquote bazel-out/darwin-fastbuild/bin -iquote external/bazel_tools -iquote bazel-out/darwin-fastbuild/bin/external/bazel_tools -no-canonical-prefixes -Wno-builtin-macro-redefined '-D__DATE__="redacted"' '-D__TIMESTAMP__="redacted"' '-D__TIME__="redacted"' '-fdebug-prefix-map=external/llvm_toolchain/=external/llvm_toolchain/' '--sysroot=/Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX10.15.sdk' -c test_bin.cpp -o bazel-out/darwin-fastbuild/bin/_objs/test_bin/test_bin.pic.o)
ERROR: /Users/chancila/Snapchat/Dev/src/broken_bazel_toolchain/BUILD.bazel:1:10: undeclared inclusion(s) in rule '//:test_bin':
this rule is missing dependency declarations for the following files included by 'test_bin.cpp':
  'bazel-out/host/bin/external/llvm_toolchain/cc_test_header.h'
Target //:test_bin failed to build
```

however using (the not recommended) `incompatible_use_specific_tool_files`

```
bazelisk run //:test_bin --incompatible_use_specific_tool_files=false
```

does not cause this issue.
