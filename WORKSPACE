workspace(name = "io_bazel_rules_go")

load("@bazel_tools//tools/build_defs/repo:http.bzl", "http_archive")
load("@io_bazel_rules_go//go:deps.bzl", "go_download_sdk", "go_register_nogo", "go_register_toolchains", "go_rules_dependencies")

# Required by toolchains_protoc.
http_archive(
    name = "platforms",
    sha256 = "218efe8ee736d26a3572663b374a253c012b716d8af0c07e842e82f238a0a7ee",
    urls = [
        "https://mirror.bazel.build/github.com/bazelbuild/platforms/releases/download/0.0.10/platforms-0.0.10.tar.gz",
        "https://github.com/bazelbuild/platforms/releases/download/0.0.10/platforms-0.0.10.tar.gz",
    ],
)

# The non-polyfill version of this is needed by rules_proto below.
http_archive(
    name = "bazel_features",
    sha256 = "d7787da289a7fb497352211ad200ec9f698822a9e0757a4976fd9f713ff372b3",
    strip_prefix = "bazel_features-1.9.1",
    url = "https://github.com/bazel-contrib/bazel_features/releases/download/v1.9.1/bazel_features-v1.9.1.tar.gz",
)

# Required by protobuf.
http_archive(
    name = "rules_cc",
    sha256 = "bbf1ae2f83305b7053b11e4467d317a7ba3517a12cef608543c1b1c5bf48a4df",
    strip_prefix = "rules_cc-0.0.16",
    urls = ["https://github.com/bazelbuild/rules_cc/releases/download/0.0.16/rules_cc-0.0.16.tar.gz"],
)

# An up-to-date version is transitively required by Stardoc to fix
# https://github.com/bazelbuild/rules_java/commit/9fd8c492e7e5751f809912554d5ee9a4cc3f53d9
http_archive(
    name = "rules_java",
    sha256 = "9b9614f8a7f7b7ed93cb7975d227ece30fe7daed2c0a76f03a5ee37f69e437de",
    urls = [
        "https://github.com/bazelbuild/rules_java/releases/download/8.3.2/rules_java-8.3.2.tar.gz",
    ],
)

load("@rules_java//java:repositories.bzl", "rules_java_dependencies", "rules_java_toolchains")

rules_java_dependencies()

rules_java_toolchains()

load("@bazel_features//:deps.bzl", "bazel_features_deps")

bazel_features_deps()

go_rules_dependencies()

go_register_toolchains(version = "1.24.0")

# Required since nogo depends on golang.org/x/tools, which needs to be at
# least version 0.30.0 to be compatible with Go 1.24, but references
# types.Info.FileVersions, which was only added to Go 1.22.

go_download_sdk(
    name = "rules_go_internal_compatibility_sdk",
    version = "1.22.12",
)

go_register_nogo(
    nogo = "@//internal:nogo",
)

# Create the host platform repository transitively required by rules_go.
load("@bazel_tools//tools/build_defs/repo:utils.bzl", "maybe")
load("@platforms//host:extension.bzl", "host_platform_repo")

maybe(
    host_platform_repo,
    name = "host_platform",
)

http_archive(
    name = "rules_proto",
    sha256 = "0e5c64a2599a6e26c6a03d6162242d231ecc0de219534c38cb4402171def21e8",
    strip_prefix = "rules_proto-7.0.2",
    url = "https://github.com/bazelbuild/rules_proto/releases/download/7.0.2/rules_proto-7.0.2.tar.gz",
)

load("@rules_proto//proto:repositories.bzl", "rules_proto_dependencies")

rules_proto_dependencies()

load("@rules_proto//proto:toolchains.bzl", "rules_proto_toolchains")

rules_proto_toolchains()

http_archive(
    name = "toolchains_protoc",
    sha256 = "f7302cce01d00c52f7ed8a033a3f133bd2c95f9608f3e4ad7d69f9e1ac2b0cc0",
    strip_prefix = "toolchains_protoc-0.3.4",
    url = "https://github.com/aspect-build/toolchains_protoc/releases/download/v0.3.4/toolchains_protoc-v0.3.4.tar.gz",
)

load("@toolchains_protoc//protoc:toolchain.bzl", "protoc_toolchains")

protoc_toolchains(
    name = "protoc_toolchains",
    version = "v25.3",
)

# An up-to-date version is required by com_google_protobuf below.
http_archive(
    name = "rules_python",
    sha256 = "ca77768989a7f311186a29747e3e95c936a41dffac779aff6b443db22290d913",
    strip_prefix = "rules_python-0.36.0",
    url = "https://github.com/bazelbuild/rules_python/releases/download/0.36.0/rules_python-0.36.0.tar.gz",
)

load("@rules_python//python:repositories.bzl", "py_repositories")

py_repositories()

http_archive(
    name = "com_google_protobuf",
    integrity = "sha256-zl0At4RQoMpAC/NgrADA1ZnMIl8EnZhqJ+mk45bFqEo=",
    strip_prefix = "protobuf-29.0-rc2",
    urls = [
        "https://github.com/protocolbuffers/protobuf/archive/v29.0-rc2.tar.gz",
        "https://mirror.bazel.build/github.com/protocolbuffers/protobuf/archive/v29.0-rc2.tar.gz",
    ],
)

load("@com_google_protobuf//:protobuf_deps.bzl", "protobuf_deps")

protobuf_deps()

# Used by //tests:buildifier_test.
http_archive(
    name = "com_github_bazelbuild_buildtools",
    sha256 = "05c3c3602d25aeda1e9dbc91d3b66e624c1f9fdadf273e5480b489e744ca7269",
    strip_prefix = "buildtools-6.4.0",
    # latest, as of 2023-11-17
    urls = ["https://github.com/bazelbuild/buildtools/archive/refs/tags/v6.4.0.tar.gz"],
)

# For manual testing against an LLVM toolchain.
# Use --extra_toolchains=@llvm_toolchain//:cc-toolchain-linux,@llvm_toolchain//:cc-toolchain-darwin
http_archive(
    name = "com_grail_bazel_toolchain",
    sha256 = "fb762268ca70ced1a0f65d24f92cd881098afd34990ae5767df0ab325217620e",
    strip_prefix = "toolchains_llvm-0.4.4",
    urls = ["https://github.com/bazel-contrib/toolchains_llvm/archive/0.4.4.tar.gz"],
)

load("@com_grail_bazel_toolchain//toolchain:rules.bzl", "llvm_toolchain")

llvm_toolchain(
    name = "llvm_toolchain",
    llvm_version = "8.0.0",
)

http_archive(
    name = "bazelci_rules",
    sha256 = "eca21884e6f66a88c358e580fd67a6b148d30ab57b1680f62a96c00f9bc6a07e",
    strip_prefix = "bazelci_rules-1.0.0",
    url = "https://github.com/bazelbuild/continuous-integration/releases/download/rules-1.0.0/bazelci_rules-1.0.0.tar.gz",
)

load("@bazelci_rules//:rbe_repo.bzl", "rbe_preconfig")

# Creates a default toolchain config for RBE.
# Use this as is if you are using the rbe_ubuntu16_04 container,
# otherwise refer to RBE docs.
rbe_preconfig(
    name = "buildkite_config",
    toolchain = "ubuntu2204",
)

# Needed for tests and tools
load("@bazel_skylib//:workspace.bzl", "bazel_skylib_workspace")

bazel_skylib_workspace()

http_archive(
    name = "bazel_gazelle",
    sha256 = "b760f7fe75173886007f7c2e616a21241208f3d90e8657dc65d36a771e916b6a",
    urls = [
        "https://mirror.bazel.build/github.com/bazelbuild/bazel-gazelle/releases/download/v0.39.1/bazel-gazelle-v0.39.1.tar.gz",
        "https://github.com/bazelbuild/bazel-gazelle/releases/download/v0.39.1/bazel-gazelle-v0.39.1.tar.gz",
    ],
)

# TODO: Move this back to the end after Gazelle updates golang.org/x/net to at least v0.26.0.
# See https://github.com/bettercap/bettercap/issues/1106 for how this breaks Go 1.23 compatibility.
load("@io_bazel_rules_go//tests:grpc_repos.bzl", "grpc_dependencies")

grpc_dependencies()

load("@bazel_gazelle//:deps.bzl", "gazelle_dependencies", "go_repository")

gazelle_dependencies(go_sdk = "go_sdk")

go_repository(
    name = "com_github_google_go_github_v36",
    importpath = "github.com/google/go-github/v36",
    sum = "h1:ndCzM616/oijwufI7nBRa+5eZHLldT+4yIB68ib5ogs=",
    version = "v36.0.0",
)

go_repository(
    name = "com_github_google_go_querystring",
    importpath = "github.com/google/go-querystring",
    sum = "h1:AnCroh3fv4ZBgVIf1Iwtovgjaw/GiKJo8M8yD/fhyJ8=",
    version = "v1.1.0",
)

go_repository(
    name = "org_golang_x_mod",
    importpath = "golang.org/x/mod",
    sum = "h1:D4nJWe9zXqHOmWqj4VMOJhvzj7bEZg4wEYa759z1pH4=",
    version = "v0.22.0",
)

go_repository(
    name = "org_golang_x_net",
    importpath = "golang.org/x/net",
    sum = "h1:P4fl1q7dY2hnZFxEk4pPSkDHF+QqjitcnDjUQyMM+pM=",
    version = "v0.31.0",
)

go_repository(
    name = "org_golang_x_sync",
    importpath = "golang.org/x/sync",
    sum = "h1:fEo0HyrW1GIgZdpbhCRO0PkJajUS5H9IFUztCgEo2jQ=",
    version = "v0.9.0",
)

go_repository(
    name = "org_golang_x_oauth2",
    importpath = "golang.org/x/oauth2",
    sum = "h1:Lh8GPgSKBfWSwFvtuWOfeI3aAAnbXTSutYxJiOJFgIw=",
    version = "v0.6.0",
)

go_repository(
    name = "org_golang_x_tools",
    importpath = "golang.org/x/tools",
    sum = "h1:qEKojBykQkQ4EynWy4S8Weg69NumxKdn40Fce3uc/8o=",
    version = "v0.27.0",
)

http_archive(
    name = "googleapis",
    sha256 = "9d1a930e767c93c825398b8f8692eca3fe353b9aaadedfbcf1fca2282c85df88",
    strip_prefix = "googleapis-64926d52febbf298cb82a8f472ade4a3969ba922",
    urls = [
        "https://github.com/googleapis/googleapis/archive/64926d52febbf298cb82a8f472ade4a3969ba922.zip",
    ],
)

go_repository(
    name = "org_golang_google_genproto",
    build_extra_args = ["-exclude=vendor"],
    build_file_generation = "on",
    build_file_proto_mode = "disable_global",
    importpath = "google.golang.org/genproto",
    sum = "h1:S9GbmC1iCgvbLyAokVCwiO6tVIrU9Y7c5oMx1V/ki/Y=",
    version = "v0.0.0-20221024183307-1bc688fe9f3e",
)

load("@io_bazel_rules_go//tests/legacy/test_chdir:remote.bzl", "test_chdir_remote")

test_chdir_remote()

load("@io_bazel_rules_go//tests/integration/popular_repos:popular_repos.bzl", "popular_repos")

popular_repos()

local_repository(
    name = "runfiles_remote_test",
    path = "tests/core/runfiles/runfiles_remote_test",
)

# For API doc generation
# This is a dev dependency, users should not need to install it
# so we declare it in the WORKSPACE
http_archive(
    name = "io_bazel_stardoc",
    sha256 = "3fd8fec4ddec3c670bd810904e2e33170bedfe12f90adf943508184be458c8bb",
    urls = [
        "https://mirror.bazel.build/github.com/bazelbuild/stardoc/releases/download/0.5.3/stardoc-0.5.3.tar.gz",
        "https://github.com/bazelbuild/stardoc/releases/download/0.5.3/stardoc-0.5.3.tar.gz",
    ],
)

load("@io_bazel_stardoc//:setup.bzl", "stardoc_repositories")

stardoc_repositories()

# For testing objc_library interop, users should not need to install it
http_archive(
    name = "build_bazel_apple_support",
    sha256 = "100d12617a84ebc7ee7a10ecf3b3e2fdadaebc167ad93a21f820a6cb60158ead",
    url = "https://github.com/bazelbuild/apple_support/releases/download/1.12.0/apple_support.1.12.0.tar.gz",
)

load(
    "@build_bazel_apple_support//lib:repositories.bzl",
    "apple_support_dependencies",
)

apple_support_dependencies()

http_archive(
    name = "rules_shell",
    sha256 = "d8cd4a3a91fc1dc68d4c7d6b655f09def109f7186437e3f50a9b60ab436a0c53",
    strip_prefix = "rules_shell-0.3.0",
    url = "https://github.com/bazelbuild/rules_shell/releases/download/v0.3.0/rules_shell-v0.3.0.tar.gz",
)

load("@rules_shell//shell:repositories.bzl", "rules_shell_dependencies", "rules_shell_toolchains")

rules_shell_dependencies()

rules_shell_toolchains()

load("@googleapis//:repository_rules.bzl", "switched_rules_by_language")

switched_rules_by_language(
    name = "com_google_googleapis_imports",
)

# For testing the compatibility with a hermetic cc toolchain. Users should not have to enable it.
http_archive(
    name = "hermetic_cc_toolchain",
    sha256 = "bd2234acd0837251361be3270d7d3ce599b418be123d902d84762302e31a3014",
    strip_prefix = "hermetic_cc_toolchain-13c904dce0cb9b6d07f0d557e6ce3cf7013a562e",
    urls = ["https://github.com/uber/hermetic_cc_toolchain/archive/13c904dce0cb9b6d07f0d557e6ce3cf7013a562e.zip"],
)

load("@hermetic_cc_toolchain//toolchain:defs.bzl", zig_toolchains = "toolchains")

zig_toolchains(
    host_platform_sha256 = {
        "linux-aarch64": "12be476ed53c219507e77737dbb7f2a77b280760b8acbc6ba2eaaeb42b7d145e",
        "linux-x86_64": "1b1c115c4ccbdc215cc3b07833c7957336d9f5fff816f97e5cafee556a9d8be8",
        "macos-aarch64": "3943612c560dd066fba5698968317a146a0f585f6cdaa1e7c1df86685c7c4eaf",
        "macos-x86_64": "0c89e5d934ecbf9f4d2dea6e3b8dfcc548a3d4184a856178b3db74e361031a2b",
    },
    version = "0.11.0-dev.3886+0c1bfe271",
)
