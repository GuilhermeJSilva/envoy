workspace(name = "envoy")

load("//bazel:api_binding.bzl", "envoy_api_binding")

envoy_api_binding()

load("//bazel:api_repositories.bzl", "envoy_api_dependencies")

envoy_api_dependencies()

load("@bazel_tools//tools/build_defs/repo:git.bzl", "git_repository")

git_repository(
    name = "v8",
    patch_args = ["-p1"],
    patch_cmds = [
        "find ./src ./include -type f -exec sed -i.bak -e 's!#include \"third_party/simdutf/simdutf.h\"!#include \"simdutf.h\"!' {} \\;",
        "find ./src ./include -type f -exec sed -i.bak -e 's!#include \"third_party/fp16/src/include/fp16.h\"!#include \"fp16.h\"!' {} \\;",
        "find ./src ./include -type f -exec sed -i.bak -e 's!#include \"third_party/dragonbox/src/include/dragonbox/dragonbox.h\"!#include \"dragonbox/dragonbox.h\"!' {} \\;",
        "find ./src ./include -type f -exec sed -i.bak -e 's!#include \"third_party/fast_float/src/include/fast_float/!#include \"fast_float/!' {} \\;",
        # TODO(jwendell): Remove the atomic_ref polyfill injection once the LLVM toolchain is
        # bumped to a version whose libc++ provides std::atomic_ref (LLVM 19+).
        "grep -rl 'std::atomic_ref' src/ include/ --include='*.h' --include='*.cc' | grep -v atomic_ref_polyfill | while IFS= read -r f; do { echo '#include \"src/base/atomic_ref_polyfill.h\"'; cat \"$f\"; } > \"$f.tmp\" && mv \"$f.tmp\" \"$f\"; done",
        # TODO(jwendell): Remove consteval->constexpr workaround once the LLVM toolchain is
        # bumped. Clang 18 has bugs with consteval in template contexts (fixed in clang 19+).
        "find ./src -type f \\( -name '*.h' -o -name '*.cc' \\) -exec sed -i.bak 's/consteval/constexpr/g' {} \\;",
        "find ./src -type f -name '*.bak' -delete",
    ],
    patches = [
        "@envoy//bazel:v8.patch",
        "@envoy//bazel:v8_atomic_ref.patch",
        "@envoy//bazel:v8_novtune.patch",
        "@envoy//bazel:v8_ppc64le.patch",
        # https://issues.chromium.org/issues/423403090
        "@envoy//bazel:v8_python.patch",
    ],
    remote = "https://github.com/v8/v8",
    tag = "14.6.202.10",
)

load("//bazel:repositories.bzl", "envoy_dependencies")

envoy_dependencies()

load("//bazel:bazel_deps.bzl", "envoy_bazel_dependencies")

envoy_bazel_dependencies()

load("//bazel:repositories_extra.bzl", "envoy_dependencies_extra")

envoy_dependencies_extra()

load("//bazel:python_dependencies.bzl", "envoy_python_dependencies")

envoy_python_dependencies()

load("//bazel:dependency_imports.bzl", "envoy_dependency_imports")

envoy_dependency_imports()

load("//bazel:repo.bzl", "envoy_repo")

envoy_repo()

load("//bazel:toolchains.bzl", "envoy_toolchains")

envoy_toolchains()

load("//bazel:dependency_imports_extra.bzl", "envoy_dependency_imports_extra")

envoy_dependency_imports_extra()
