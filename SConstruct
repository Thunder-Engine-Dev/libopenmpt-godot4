#!/usr/bin/env python3

import os
import sys
import scons_compiledb

env = SConscript("godot-cpp/SConstruct")


extension_api_builder = Builder(action = "godot --dump-extension-api $TARGET")

# Gets the standard flags CC, CCX, etc.
scons_compiledb.enable_with_cmdline(env)

# Define the relative path to the Godot headers.
godot_path = "godot-cpp"
target_name = "libopenmptgodot"

# libopenmpt-specific variables
# The windows version is based on the official libopenmpt distribution
# The linux version is based on direct compilation of the library.
if env["platform"] == "windows":
    libopenmpt_path = "deps/libopenmpt"
    #libopenmpt_headers_path = libopenmpt_path + "/inc"
    libopenmpt_lib_path = libopenmpt_path + "/lib"
else:
    libopenmpt_path = "deps/libopenmpt/install-files"
    #libopenmpt_headers_path = libopenmpt_path + "/include"
    libopenmpt_lib_path = libopenmpt_path + "/lib"

env.Append(
    CPPPATH=[
        "src/",
        libopenmpt_path + "/include",
        godot_path + "/gdextension",
        godot_path + "/include",
        godot_path + "/include/godot_cpp/classes",
        godot_path + "/include/godot_cpp/core",
        godot_path + "/include/godot_cpp/variant",
        godot_path + "/gen/include",
        godot_path + "/gen/include/godot_cpp/classes",
        godot_path + "/gen/include/godot_cpp/core",
        godot_path + "/gen/include/godot_cpp/variant",
    ]
)

env.Append(
    LIBPATH=[
        godot_path + "/bin/",
        libopenmpt_path + "/lib"
    ]
)

# TODO make the pathing dynamic here
if env["platform"] == "windows":
    env.Append(
        LIBS=[
            env.File(os.path.join(godot_path + "/bin", "libgodot-cpp.%s.%s.%s%s" %
                     (env["platform"], env["target"], env["arch"], env["LIBSUFFIX"]))),
            "amd64/libopenmpt"
        ]
    )
    env.Append(CCFLAGS=['-EHsc'])
    env.Append(CCFLAGS=['-Wall', '-std=17', '-pedantic'])
else:
    env.Append(
        LIBS=[
            env.File(os.path.join(godot_path + "/bin", "libgodot-cpp.%s.%s.%s%s" %
                     (env["platform"], env["target"], env["arch"], env["LIBSUFFIX"]))),
            "openmpt", "z", "mpg123", "vorbisfile", "vorbis", "m", "ogg"
        ]
    )

#env.Append(CPPPATH=["src/"])
sources = Glob("src/*.cpp")
# def add_sources(sources, dir):
    # for f in os.listdir(dir):
        # if f.endswith(".cpp"):
            # sources.append(dir + "/" + f)
            
# sources = []
# add_sources(sources, "src")

if env["platform"] == "macos":
    library = env.SharedLibrary(
        os.path.join(libopenmpt_path, "%s.%s.%s.framework/%s.%s.%s" %
            (target_name, env["platform"], env["target"], target_name, env["platform"], env["target"])),
        source=sources,
    )
else:
    library = env.SharedLibrary(target=
        os.path.join(libopenmpt_path, "%s.%s.%s.%s%s" %
                     (target_name, env["platform"], env["target"], env["arch"], env["SHLIBSUFFIX"])
        ), source=sources)


# if env["platform"] == "macos":
#     library = env.SharedLibrary(
#         os.path.join(env["target_path"], "%s.%s.%s.framework/libgdexample.%s.%s" %
#             (env["platform"], env["target"], env["platform"], env["target"])),
#         source=sources,
#     )
# else:
#     library = env.SharedLibrary(target=
#         os.path.join(env["target_path"], "libgodot-cpp.%s.%s.%s%s" %
#                      (platform, env["target"], env["arch"], env["SHLIBSUFFIX"])), source=sources)


Default(library)
