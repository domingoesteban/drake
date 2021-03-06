# -*- python -*-

load("@bazel_tools//tools/build_defs/pkg:pkg.bzl", "pkg_tar")
load(
    "@drake//tools:install.bzl",
    "cmake_config",
    "install",
    "install_cmake_config",
)
load("@drake//tools:python_lint.bzl", "python_lint")

package(default_visibility = ["//visibility:public"])

# Note that this is only a portion of Bullet.

BULLET_COPTS = ["-Wno-all"]

BULLET_INCLUDES = ["src"]

# Keep defines in sync with Components.LinearMath.Definitions in
# bullet-create-cps.py.
cc_library(
    name = "LinearMath",
    srcs = glob(["src/LinearMath/**/*.cpp"]),
    hdrs = glob(["src/LinearMath/**/*.h"]),
    copts = BULLET_COPTS,
    defines = ["BT_USE_DOUBLE_PRECISION"],
    includes = BULLET_INCLUDES,
)

cc_library(
    name = "BulletCollision",
    srcs = glob(["src/BulletCollision/**/*.cpp"]),
    hdrs = glob(["src/BulletCollision/**/*.h"]) + [
        "src/btBulletCollisionCommon.h",
    ],
    copts = BULLET_COPTS,
    includes = BULLET_INCLUDES,
    deps = [":LinearMath"],
)

cc_library(
    name = "BulletDynamics",
    srcs = glob(["src/BulletDynamics/**/*.cpp"]),
    hdrs = glob(["src/BulletDynamics/**/*.h"]) + [
        "src/btBulletDynamicsCommon.h",
    ],
    copts = BULLET_COPTS,
    includes = BULLET_INCLUDES,
    deps = [
        ":BulletCollision",
        ":LinearMath",
    ],
)

cc_library(
    name = "BulletSoftBody",
    srcs = glob(["src/BulletSoftBody/**/*.cpp"]),
    hdrs = glob(["src/BulletSoftBody/**/*.h"]),
    copts = BULLET_COPTS,
    includes = BULLET_INCLUDES,
    deps = [
        ":BulletCollision",
        ":BulletDynamics",
        ":LinearMath",
    ],
)

cmake_config(
    package = "Bullet",
    script = "@drake//tools:bullet-create-cps.py",
    version_file = "VERSION",
)

install_cmake_config(package = "Bullet")

install(
    name = "install",
    docs = ["AUTHORS.txt"],
    guess_hdrs = "PACKAGE",
    hdr_dest = "include/bullet",
    hdr_strip_prefix = BULLET_INCLUDES,
    license_docs = ["LICENSE.txt"],
    targets = [
        ":BulletCollision",
        ":BulletDynamics",
        ":BulletSoftBody",
        ":LinearMath",
    ],
    deps = [":install_cmake_config"],
)

python_lint()
