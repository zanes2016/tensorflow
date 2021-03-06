load("@io_bazel_rules_go//go:def.bzl", "go_library", "go_test")

licenses(["notice"])  # Apache 2.0

exports_files(["LICENSE"])

sh_test(
    name = "test",
    size = "small",
    srcs = ["test.sh"],
    data = [
        ":all_files",  # Go sources
        "//tensorflow:libtensorflow.so",  # C library
        "//tensorflow/c:headers",  # C library header
        "//tensorflow/cc/saved_model:saved_model_half_plus_two",  # Testdata for LoadSavedModel
    ],
)

filegroup(
    name = "all_files",
    srcs = glob(
        ["**/*"],
        exclude = [
            "**/METADATA",
            "**/OWNERS",
        ],
    ),
    visibility = ["//tensorflow:__subpackages__"],
)

cc_import(
  name = "libtensorflow.so",
  shared_library = "//tensorflow:libtensorflow.so",
)

cc_import(
  name = "libtensorflow_framework.so",
  shared_library = "//tensorflow:libtensorflow_framework.so",
)

go_library(
    name = "go_default_library",
    srcs = [
        "doc.go",
        "graph.go",
        "lib.go",
        "operation.go",
        "saved_model.go",
        "session.go",
        "shape.go",
        "status.go",
        "tensor.go",
        "version.go",
        "//tensorflow/c:headers",  # C library header
    ],
    cdeps = [
      ":libtensorflow_framework.so",
      ":libtensorflow.so",  # C library
    ],
    cgo = True,
    copts = ["-Itensorflow/go"],
    importpath = "github.com/tensorflow/tensorflow/tensorflow/go",
    visibility = ["//visibility:public"],
)

go_test(
    name = "go_default_test",
    srcs = [
        "graph_test.go",
        "operation_test.go",
        "saved_model_test.go",
        "session_test.go",
        "shape_test.go",
        "tensor_test.go",
        "util_test.go",
    ],
    data = [
        "//tensorflow/cc/saved_model:saved_model_half_plus_two",  # Testdata for LoadSavedModel
    ],
    embed = [":go_default_library"],
)

go_test(
    name = "go_default_xtest",
    srcs = ["example_inception_inference_test.go"],
    deps = [
        "//tensorflow/go:go_default_library",
        "//tensorflow/go/op:go_default_library",
    ],
)
