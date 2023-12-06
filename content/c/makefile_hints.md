Title: Unset Title
Date: 2023-12-06
Tags: c, makefile
Summary: In the process of the senior project, I learned more things about makefiles

Longer description TODO

-------


### Conditional Blocks ([stackoverflow](https://stackoverflow.com/questions/6269857/makefile-support-for-multiple-architectures-configurations))

```make
ifeq ($(TARGET), scmb)
  CC = gcc-riscv-eabi-none
else
  CC = gcc
endif
```

In a style like you would expect from a scripting language, defining the variable inside the `if` doesn't prevent it from being accessible later. However, if you want to indent inside the `if`, make sure that it doesn't match the indentation used for commands inside a rule.

### Automatically Build Header File Dependencies ([stackoverflow](https://stackoverflow.com/questions/2394609/makefile-header-dependencies))

```make
CXX = clang++
CXX_FLAGS = -Wfatal-errors -Wall -Wextra -Wpedantic -Wconversion -Wshadow

# Final binary
BIN = mybin
# Put all auto generated stuff to this build dir.
BUILD_DIR = ./build

# List of all .cpp source files.
CPP = main.cpp $(wildcard dir1/*.cpp) $(wildcard dir2/*.cpp)

# All .o files go to build dir.
OBJ = $(CPP:%.cpp=$(BUILD_DIR)/%.o)
# Gcc/Clang will create these .d files containing dependencies.
DEP = $(OBJ:%.o=%.d)

# Default target named after the binary.
$(BIN) : $(BUILD_DIR)/$(BIN)

# Actual target of the binary - depends on all .o files.
$(BUILD_DIR)/$(BIN) : $(OBJ)
    # Create build directories - same structure as sources.
    mkdir -p $(@D)
    # Just link all the object files.
    $(CXX) $(CXX_FLAGS) $^ -o $@

# Include all .d files
-include $(DEP)

# Build target for every single object file.
# The potential dependency on header files is covered
# by calling `-include $(DEP)`.
$(BUILD_DIR)/%.o : %.cpp
    mkdir -p $(@D)
    # The -MMD flags additionaly creates a .d file with
    # the same name as the .o file.
    $(CXX) $(CXX_FLAGS) -MMD -c $< -o $@

.PHONY : clean
clean :
    # This should remove all generated files.
    -rm $(BUILD_DIR)/$(BIN) $(OBJ) $(DEP)
```

The important parts here are building the `DEP` list of `.d` files, and the `-MMD` flag to gcc in the rule for building `.o` files. This snippet also makes use of the `-p` flag to `mkdir` to not error on the directory already existing, but I prefer having a dedicated rule for `mkdir`, and having the other rules having the timeless dependency, like `%.o: %.c | $(BUILD_DIR)`