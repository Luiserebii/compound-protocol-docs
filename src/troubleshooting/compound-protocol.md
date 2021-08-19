# compound-protocol

## `Unknown option: p`

When running one of the package scripts, like `yarn test`, the output errors and stops, displaying:

```
Unknown option: p
Type shasum -h for help
Unknown option: p
Type shasum -h for help
error Command failed with exit code 1.
info Visit https://yarnpkg.com/en/docs/cli/run for documentation about this command.
```

As of the commit written to this guide, this fix has been merged as of [eb7a6c9](https://github.com/compound-finance/compound-protocol/commit/eb7a6c9831198a19736bc4c1f8f66d41b98f4eaf), but as the error appeared during the writing of this guide, it will be mentioned. Chances are, if you have this error, you have checked out an earlier commit of Compound protocol.

To fix this error which is relevant to a newer version of `shasum`, make the changes to the files described in this pull request: [https://github.com/compound-finance/compound-protocol/pull/80/files](https://github.com/compound-finance/compound-protocol/pull/80/files).

## `Error: Source file requires different compiler version (current compiler is x.x.x+commit.xxxxxxxx.xxx.xxx)`

`solc` v0.5.16+ is required to compile the current smart contracts. The solution is to install that version of `solc` to your computer. To install, please see the [Installation section](../getting-started.md#solc-v0516).

## `TypeError: Object.fromEntries is not a function`

If you're getting this error, you are likely on an older version of node (check `node -v`). Chances are, you have installed node from the `apt` package manager, which installs Node 10.x. Due to the requirement of a version of Node below 14+ for `compound-eureka`, I recommend that you install Node 13.x. To install, please see the [Installation section](../getting-started.md#node-13x).

## `../libusb/libusb/os/linux_udev.c:40:10: fatal error: libudev.h: No such file or directory`

This error may appear when installing packages in the initial setup phase. The full error output may look like:

```sh
root@ubuntu-s-2vcpu-4gb-intel-tor1-01:~/compound-protocol# yarn install --lock-file
yarn install v1.22.11
[1/4] Resolving packages...
[2/4] Fetching packages...
info fsevents@1.2.11: The platform "linux" is incompatible with this module.
info "fsevents@1.2.11" is an optional dependency and failed compatibility check. Excluding it from installation.
info fsevents@2.1.2: The platform "linux" is incompatible with this module.
info "fsevents@2.1.2" is an optional dependency and failed compatibility check. Excluding it from installation.
[3/4] Linking dependencies...
warning "eth-saddle > web3-provider-engine > eth-block-tracker > @babel/plugin-transform-runtime@7.8.3" has unmet peer dependency "@babel/core@^7.0.0-0".
[4/4] Building fresh packages...
[11/32] ⡀ usb
[10/32] ⡀ node-hid
[3/32] ⢀ sha3
[4/32] ⢀ keccak
warning Error running install script for optional dependency: "/root/compound-protocol/node_modules/node-hid: Command failed.
Exit code: 1
Command: prebuild-install || node-gyp rebuild
Arguments:
Directory: /root/compound-protocol/node_modules/node-hid
Output:
prebuild-install WARN install No prebuilt binaries found (target=14.17.4 runtime=node arch=x64 libc= platform=linux)
gyp info it worked if it ends with ok
gyp info using node-gyp@5.1.0
gyp info using node@14.17.4 | linux | x64
gyp info find Python using Python version 3.8.5 found at \"/usr/bin/python3\"
gyp info spawn /usr/bin/python3
gyp info spawn args [
gyp info spawn args   '/usr/lib/node_modules/npm/node_modules/node-gyp/gyp/gyp_main.py',
gyp info spawn args   'binding.gyp',
gyp info spawn args   '-f',
gyp info spawn args   'make',
gyp info spawn args   '-I',
gyp info spawn args   '/root/compound-protocol/node_modules/node-hid/build/config.gypi',
gyp info spawn args   '-I',
gyp info spawn args   '/usr/lib/node_modules/npm/node_modules/node-gyp/addon.gypi',
gyp info spawn args   '-I',
gyp info spawn args   '/root/.cache/node-gyp/14.17.4/include/node/common.gypi',
gyp info spawn args   '-Dlibrary=shared_library',
gyp info spawn args   '-Dvisibility=default',
gyp info spawn args   '-Dnode_root_dir=/root/.cache/node-gyp/14.17.4',
gyp info spawn args   '-Dnode_gyp_dir=/usr/lib/node_modules/npm/node_modules/node-gyp',
gyp info spawn args   '-Dnode_lib_file=/root/.cache/node-gyp/14.17.4/<(target_arch)/node.lib',
gyp info spawn args   '-Dmodule_root_dir=/root/compound-protocol/node_modules/node-hid',
gyp info spawn args   '-Dnode_engine=v8',
gyp info spawn args   '--depth=.',
gyp info spawn args   '--no-parallel',
gyp info spawn args   '--generator-output',
gyp info spawn args   'build',
gyp info spawn args   '-Goutput_dir=.'
gyp info spawn args ]
gyp info spawn make
gyp info spawn args [ 'BUILDTYPE=Release', '-C', 'build' ]
make: Entering directory '/root/compound-protocol/node_modules/node-hid/build'
  CC(target) Release/obj.target/hidapi-linux-hidraw/hidapi/linux/hid.o
../hidapi/linux/hid.c:44:10: fatal error: libudev.h: No such file or directory
   44 | #include <libudev.h>
      |          ^~~~~~~~~~~
compilation terminated.
make: *** [hidapi-linux-hidraw.target.mk:111: Release/obj.target/hidapi-linux-hidraw/hidapi/linux/hid.o] Error 1
make: Leaving directory '/root/compound-protocol/node_modules/node-hid/build'
gyp ERR! build error
gyp ERR! stack Error: `make` failed with exit code: 2
gyp ERR! stack     at ChildProcess.onExit (/usr/lib/node_modules/npm/node_modules/node-gyp/lib/build.js:194:23)
gyp ERR! stack     at ChildProcess.emit (events.js:400:28)
gyp ERR! stack     at Process.ChildProcess._handle.onexit (internal/child_process.js:277:12)
gyp ERR! System Linux 5.4.0-73-generic
gyp ERR! command \"/usr/bin/node\" \"/usr/lib/node_modules/npm/node_modules/node-gyp/bin/node-gyp.js\" \"rebuild\"
gyp ERR! cwd /root/compound-protocol/node_modules/node-hid
[11/32] ⠄ usb
[12/32] ⠄ websocket
[-/32] ⡀ waiting...
[-/32] ⡀ waiting...
warning Error running install script for optional dependency: "/root/compound-protocol/node_modules/usb: Command failed.
Exit code: 1
Command: prebuild-install --verbose || node-gyp rebuild
Arguments:
Directory: /root/compound-protocol/node_modules/usb
Output:
prebuild-install info begin Prebuild-install version 5.3.3
prebuild-install info looking for cached prebuild @ /root/.npm/_prebuilds/f28b5a-usb-v1.6.2-node-v83-linux-x64.tar.gz
prebuild-install http request GET https://github.com/tessel/node-usb/releases/download/v1.6.2/usb-v1.6.2-node-v83-linux-x64.tar.gz
prebuild-install http 404 https://github.com/tessel/node-usb/releases/download/v1.6.2/usb-v1.6.2-node-v83-linux-x64.tar.gz
prebuild-install WARN install No prebuilt binaries found (target=14.17.4 runtime=node arch=x64 libc= platform=linux)
gyp info it worked if it ends with ok
gyp info using node-gyp@5.1.0
gyp info using node@14.17.4 | linux | x64
gyp info find Python using Python version 3.8.5 found at \"/usr/bin/python3\"
gyp info spawn /usr/bin/python3
gyp info spawn args [
gyp info spawn args   '/usr/lib/node_modules/npm/node_modules/node-gyp/gyp/gyp_main.py',
gyp info spawn args   'binding.gyp',
gyp info spawn args   '-f',
gyp info spawn args   'make',
gyp info spawn args   '-I',
gyp info spawn args   '/root/compound-protocol/node_modules/usb/build/config.gypi',
gyp info spawn args   '-I',
gyp info spawn args   '/usr/lib/node_modules/npm/node_modules/node-gyp/addon.gypi',
gyp info spawn args   '-I',
gyp info spawn args   '/root/.cache/node-gyp/14.17.4/include/node/common.gypi',
gyp info spawn args   '-Dlibrary=shared_library',
gyp info spawn args   '-Dvisibility=default',
gyp info spawn args   '-Dnode_root_dir=/root/.cache/node-gyp/14.17.4',
gyp info spawn args   '-Dnode_gyp_dir=/usr/lib/node_modules/npm/node_modules/node-gyp',
gyp info spawn args   '-Dnode_lib_file=/root/.cache/node-gyp/14.17.4/<(target_arch)/node.lib',
gyp info spawn args   '-Dmodule_root_dir=/root/compound-protocol/node_modules/usb',
gyp info spawn args   '-Dnode_engine=v8',
gyp info spawn args   '--depth=.',
gyp info spawn args   '--no-parallel',
gyp info spawn args   '--generator-output',
gyp info spawn args   'build',
gyp info spawn args   '-Goutput_dir=.'
gyp info spawn args ]
gyp info spawn make
gyp info spawn args [ 'BUILDTYPE=Release', '-C', 'build' ]
make: Entering directory '/root/compound-protocol/node_modules/usb/build'
  CC(target) Release/obj.target/libusb/libusb/libusb/core.o
  CC(target) Release/obj.target/libusb/libusb/libusb/descriptor.o
  CC(target) Release/obj.target/libusb/libusb/libusb/hotplug.o
  CC(target) Release/obj.target/libusb/libusb/libusb/io.o
  CC(target) Release/obj.target/libusb/libusb/libusb/strerror.o
  CC(target) Release/obj.target/libusb/libusb/libusb/sync.o
  CC(target) Release/obj.target/libusb/libusb/libusb/os/poll_posix.o
  CC(target) Release/obj.target/libusb/libusb/libusb/os/threads_posix.o
  CC(target) Release/obj.target/libusb/libusb/libusb/os/linux_usbfs.o
  CC(target) Release/obj.target/libusb/libusb/libusb/os/linux_udev.o
../libusb/libusb/os/linux_udev.c:40:10: fatal error: libudev.h: No such file or directory
   40 | #include <libudev.h>
      |          ^~~~~~~~~~~
compilation terminated.
make: *** [libusb.target.mk:150: Release/obj.target/libusb/libusb/libusb/os/linux_udev.o] Error 1
make: Leaving directory '/root/compound-protocol/node_modules/usb/build'
gyp ERR! build error
gyp ERR! stack Error: `make` failed with exit code: 2
gyp ERR! stack     at ChildProcess.onExit (/usr/lib/node_modules/npm/node_modules/node-gyp/lib/build.js:194:23)
gyp ERR! stack     at ChildProcess.emit (events.js:400:28)
gyp ERR! stack     at Process.ChildProcess._handle.onexit (internal/child_process.js:277:12)
gyp ERR! System Linux 5.4.0-73-generic
gyp ERR! command \"/usr/bin/node\" \"/usr/lib/node_modules/npm/node_modules/node-gyp/bin/node-gyp.js\" \"rebuild\"
gyp ERR! cwd /root/compound-protocol/node_modules/usb
Done in 55.61s.
```

What you will want to do is to make sure `libusb` and `libudev` are installed. To install them and fix the error, please see the [Installation section](../getting-started.md#libusb-and-libudev).

**NOTE**: If you have run a command that invokes this error, it is expected for the command to display this error at least once after installing, so don't worry if you see it again! Simply try the command again, and it should run successfully.

## `Error: Callback was already called`

This error occurs with Node 14+. To solve, install an earlier version of Node, preferably Node 13.x. To install, please see the [Installation section](../getting-started.md#node-13x).
