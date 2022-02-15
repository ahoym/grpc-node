[![Build Status](https://travis-ci.org/grpc/grpc-node.svg?branch=master)](https://travis-ci.org/grpc/grpc-node)
# gRPC on Node.js

## Implementations

For a comparison of the features available in these two libraries, see [this document](https://github.com/grpc/grpc-node/tree/master/PACKAGE-COMPARISON.md)

### C-based Client and Server

Directory: [`packages/grpc-native-core`](https://github.com/grpc/grpc-node/tree/grpc@1.24.x/packages/grpc-native-core) (lives in the `grpc@1.24.x` branch) (see here for installation information)

npm package: [grpc](https://www.npmjs.com/package/grpc).

This is the existing, feature-rich implementation of gRPC using a C++ addon. It works on all LTS versions of Node.js on most platforms that Node.js runs on.

### Pure JavaScript Client

Directory: [`packages/grpc-js`](https://github.com/grpc/grpc-node/tree/master/packages/grpc-js)

npm package: [@grpc/grpc-js](https://www.npmjs.com/package/@grpc/grpc-js)

This library implements the core functionality of gRPC purely in JavaScript, without a C++ addon. It works on the latest version of Node.js on all platforms that Node.js runs on.

## Other Packages

### gRPC Protobuf Loader

Directory: [`packages/proto-loader`](https://github.com/grpc/grpc-node/tree/master/packages/proto-loader)

npm package: [@grpc/proto-loader](https://www.npmjs.com/package/@grpc/proto-loader)

This library loads `.proto` files into objects that can be passed to the gRPC libraries.

### gRPC Tools

Directory: [`packages/grpc-tools`](https://github.com/grpc/grpc-node/tree/master/packages/grpc-tools)

npm package: [grpc-tools](https://www.npmjs.com/package/grpc-tools)

Distribution of protoc and the gRPC Node protoc plugin for ease of installation with npm.

### gRPC Health Check Service

Directory: [`packages/grpc-health-check`](https://github.com/grpc/grpc-node/tree/master/packages/grpc-health-check)

npm package: [grpc-health-check](https://www.npmjs.com/package/grpc-health-check)

Health check service for gRPC servers.

### Building gprc-tools

**All Credit to: https://github.com/grpc/grpc-node/compare/maschwenk:grpc-tools%401.11.2-apple-silicon-build#diff-b335630551682c19a781afebcf4d07bf978fb1f8ac04c6bf87428ed5106870f5R50**

The Apple M1 Macbooks have a different OS architecture (arm64) than previous macbooks (x64).
The arm64 binary is not provided out of the box by `grpc-tools`, so we need to manually build it.

If `grpc-tools` _does_ decide to bundle the arm64 binaries natively, then STOP USING THIS FORK.
See: https://github.com/grpc/grpc-node/issues/1405 for status.

Note that any changes to [the `grpc-node` parent repo](https://github.com/grpc/grpc-node) may need to result in a re-compilation of the binaries.

Steps:

```sh
git clone git@github.com:grpc/grpc-node.git
cd grpc-node
brew install cmake # ./build_binaries.sh needs cmake. also tried xcode-select --install but it didn't seem to work
cd packages/grpc-tools && git submodule update --init --recursive && ./build_binaries.sh

# git add ../../artifacts
# git commit


# then to install in pnpm (does not support npm flag)
npm_config_grpc_tools_binary_host_mirror="https://github.com/ahoym/grpc-node/raw/cc9e8ba469d0d48561b0d3f369482a29262c363e/artifacts/" yarn install
# checkout https://github.com/mapbox/node-pre-gyp/blob/master/lib/util/versioning.js#L316
# seems passing as an ENV also works (so will probably work for PNPM and Yarn)
```