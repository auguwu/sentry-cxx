<img align="right" src="assets/sentry.svg" width="196" height="196" draggable=false />

### 🐻‍❄️💤 Sentry for C++20
#### *A higher-level Sentry SDK for C++20 projects*
**sentry-cxx** is a project built to provide a higher-level [Sentry](https://sentry.io) SDK for C++20 projects that uses the experimental [`sentry-native`] project that supports CMake, Bazel, and Meson as the build systems of choice to support.

> [!CAUTION]
> This project is NOT an official Sentry SDK. Please do not bother the maintainers of `sentry-native` with issues that this library has caused.
>
> This project is independently maintained.

[`sentry-native`]: https://github.com/getsentry/sentry-native

## Usage
**sentry-cxx** aims to support the three major build systems:

* CMake 3.25+
* Bazel 7.6+
* Meson 1.1+

At the moment, Bazel is the most feature complete and compiles correctly. Meson and CMake are not on the target for a MVP right now, but I aim to support both systems in the future.

```cpp
// This library uses `noel/` as the main entrypoint so that it doesn't conflict with `sentry.h`. You can still
// include `sentry.h` but it is recommended to have `noel/Sentry.h` or use any header from `Noel/Sentry/{name}.h`
#include <noel/Sentry.h>

int main() {
    sentry::Dsn dsn = sentry::Dsn::Parse("https://examplePublicKey@o0.ingest.sentry.io/0")
        .Expect("failed to construct dsn");

    // When a `client` is built, it acts like a RAII guard. It'll call `sentry_close`
    // when it hits out of scope.
    sentry::Client client = sentry::ClientOptions(dsn)
        .DatabasePath(".sentry-native")
        .Release("my-project-name@2.3.12")
        .Debug()
        .Build();

    // Now a client is avaliable, we can send a message to Sentry
    client.CaptureEvent(
        sentry::Event::Message(sentry::EventLevel::Info)
            .Logger("custom")
            .Message("it works!")
    );
}
```

### Bazel
<!--
At the moment, `sentry-cxx` is not published on the BCR. You can use the mirror located `bzl.floofy.dev` as a registry to get clear updates like so:

```bazelrc
build --registry=https://bzl.floofy.dev

# This is required if you plan on using the registry version
build --registry=https://bcr.bazel.build
```
-->

```python
bazel_dep(name = "sentry", version = "0.12.2")
git_override(
    module_name = "sentry",
    remote = "https://github.com/auguwu/sentry-cxx.git",
    commit = "<pinned commit>"
)
```

### CMake
Not supported yet.

### Meson
Not supported yet.

## License
**sentry-cxx** is released under the **MIT License** with love and care by [Noel Towa](https://floofy.dev)! :polar_bear::pink_heart:

*Sentry (branding, company) is a registered Trademark of Functional Software, Inc.*
