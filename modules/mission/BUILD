load("@rules_cc//cc:defs.bzl", "cc_binary", "cc_library", "cc_test")
load("@rules_pkg//pkg:install.bzl", "pkg_install")
load("@rules_pkg//pkg:mappings.bzl", "pkg_attributes", "pkg_filegroup", "pkg_files", "strip_prefix")

package(default_visibility = ["//visibility:public"])

MISSION_COPTS = ['-DMODULE_NAME=\\"mission\\"']

cc_library(
    name = "mission_component_lib",
    srcs = ["mission_component.cc"],
    hdrs = ["mission_component.h"],
    copts = MISSION_COPTS,
    deps = [
        "//modules/mission/common:mission_context",
        "//modules/mission/common:mission_request_contract",
        "//modules/mission/nodes/action",
        "//modules/mission/nodes/condition",
        "//modules/mission/proto:mission_config_cc_proto",
        "@behaviortree_cpp",
        "@core//cyber",
        "@tinyxml2",
        "@wheelos_common//execution_state:flags",
        "@wheelos_common//modules/common/adapters:adapter_gflags",
        "@wheelos_msgs//wheelos_msgs/basic_msgs:geometry_cc_proto",
        "@wheelos_msgs//wheelos_msgs/chassis_msgs:chassis_cc_proto",
        "@wheelos_msgs//wheelos_msgs/localization_msgs:localization_cc_proto",
        "@wheelos_msgs//wheelos_msgs/mission_msgs:mission_request_cc_proto",
        "@wheelos_msgs//wheelos_msgs/mission_msgs:mission_request_result_cc_proto",
        "@wheelos_msgs//wheelos_msgs/mission_msgs:mission_runtime_status_cc_proto",
        "@wheelos_msgs//wheelos_msgs/planning_msgs:mission_directive_cc_proto",
        "@wheelos_msgs//wheelos_msgs/planning_msgs:planning_command_cc_proto",
    ],
    alwayslink = True,
)

cc_binary(
    name = "libmission_component.so",
    linkshared = True,
    linkstatic = False,
    deps = [":mission_component_lib"],
)

cc_test(
    name = "mission_lifecycle_test",
    size = "small",
    srcs = ["mission_lifecycle_test.cc"],
    deps = [
        ":mission_component_lib",
        "//modules/mission/common:mission_context",
        "//modules/mission/nodes/action:move_to_node",
        "//modules/mission/nodes/action:park_in_node",
        "@behaviortree_cpp",
        "@com_google_googletest//:gtest_main",
        "@core//cyber",
        "@wheelos_common//execution_state:flags",
        "@wheelos_common//execution_state:store",
        "@wheelos_common//modules/common/adapters:adapter_gflags",
        "@wheelos_msgs//wheelos_msgs/control_msgs:safety_stop_cc_proto",
        "@wheelos_msgs//wheelos_msgs/planning_msgs:planning_runtime_status_cc_proto",
    ],
)

filegroup(
    name = "runtime_data",
    srcs = glob([
        "conf/**",
        "dag/*.dag",
        "launch/*.launch",
    ]),
)

pkg_files(
    name = "package_files_data",
    srcs = [":runtime_data"],
    attributes = pkg_attributes(mode = "0644"),
    prefix = "mission",
    strip_prefix = strip_prefix.from_root("modules/mission"),
)

pkg_files(
    name = "package_files_libraries",
    srcs = [":libmission_component.so"],
    attributes = pkg_attributes(mode = "0755"),
    prefix = "mission/lib",
    strip_prefix = strip_prefix.from_root("modules/mission"),
)

pkg_filegroup(
    name = "package_files",
    srcs = [
        ":package_files_data",
        ":package_files_libraries",
    ],
)

pkg_install(
    name = "install_dir",
    srcs = [":package_files"],
    tags = ["manual"],
)
