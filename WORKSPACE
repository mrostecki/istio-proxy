# Copyright 2016 Google Inc. All Rights Reserved.
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#    http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
#
################################################################################
#

load(
     "//:repositories.bzl",
     "googletest_repositories",
     "mixerapi_dependencies",
)
load("@bazel_tools//tools/build_defs/repo:http.bzl", "http_archive")

googletest_repositories()
mixerapi_dependencies()

bind(
    name = "boringssl_crypto",
    actual = "//external:ssl",
)

# When updating envoy sha manually please update the sha in istio.deps file also
ENVOY_SHA = "74de08a0d4d31bd466639d25d681df5d290bb770"
ENVOY_SHA256 = "12905d063be326a2f6e60b9d3448772e5a163794e7d7e46276b29c9565fe0365"

http_archive(
    name = "envoy",
    strip_prefix = "envoy-" + ENVOY_SHA,
    url = "https://github.com/envoyproxy/envoy/archive/" + ENVOY_SHA + ".zip",
    sha256 = ENVOY_SHA256,
    patches = [
        "//:envoy-proxy-link-third-party-libraries-dynamically.patch",
        "//:envoy-proxy-unbundle-dependencies.patch",
        "//:envoy-proxy-use-go-sdk-from-host.patch",
        "//:envoy-proxy-change-boringssl-headers-path.patch",
    ],
)

load("@envoy//bazel:repositories.bzl", "envoy_dependencies")
envoy_dependencies()

load("@envoy//bazel:cc_configure.bzl", "cc_configure")
cc_configure()

load("@envoy_api//bazel:repositories.bzl", "api_dependencies")
api_dependencies()

load("@io_bazel_rules_go//go:def.bzl", "go_rules_dependencies", "go_register_toolchains")
go_rules_dependencies()
go_register_toolchains()

# Nov 28, 2017 (bazel 0.8.0 support)
RULES_PROTOBUF_SHA = "563b674a2ce6650d459732932ea2bc98c9c9a9bf"
RULES_PROTOBUF_SHA256 = "338e0d65cd709c6a6f9b5702466e641d536479be8b564d1e12a5d1de22a5cff6"

http_archive(
    name = "org_pubref_rules_protobuf",
    strip_prefix = "rules_protobuf-" + RULES_PROTOBUF_SHA,
    url = "https://github.com/pubref/rules_protobuf/archive/" + RULES_PROTOBUF_SHA + ".tar.gz",
    sha256 = RULES_PROTOBUF_SHA256,
)
