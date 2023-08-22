# -*- python -*-
#
# Copyright 2022 Stéphane Caron

load("//tools/lint:lint.bzl", "add_lint_tests")
load("@pip_upkie//:requirements.bzl", "requirement")

package(default_visibility = ["//visibility:public"])

py_library(
    name = "rewards",
    srcs = [
        "reward.py",
        "survival_reward.py",
    ],
    deps = [
        requirement("numpy"),
    ],
)

py_library(
    name = "upkie_base_env",
    srcs = [
        "reward.py",
        "upkie_base_env.py",
    ],
    deps = [
        "//upkie/config",
        "//upkie/observers/base_pitch",
        ":rewards",
        "@vulp//spine:python",
        requirement("gymnasium"),
        requirement("loop_rate_limiters"),
        requirement("upkie_description"),
    ],
)

py_library(
    name = "upkie_servos_env",
    srcs = [
        "upkie_servos_env.py",
    ],
    deps = [
        ":upkie_base_env",
        "//upkie/utils:pinocchio",
    ],
)

py_library(
    name = "upkie_wheels_env",
    srcs = [
        "upkie_wheels_env.py",
    ],
    deps = [
        ":upkie_base_env",
    ],
)

py_library(
    name = "envs",
    srcs = [
        "__init__.py",
    ],
    deps = [
        ":rewards",
        ":upkie_base_env",
        ":upkie_servos_env",
        ":upkie_wheels_env",
        requirement("gymnasium"),
    ],
)

add_lint_tests()