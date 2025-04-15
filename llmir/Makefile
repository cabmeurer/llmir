# Makefile for llmir project

PROJECT=llmir
CONFIGURATION=Debug

# Find the DerivedData directory with our build
DERIVED_DATA_DIR=$(shell find ~/Library/Developer/Xcode/DerivedData -name "llmir-*" -type d | head -n 1)
EXECUTABLE_PATH=$(DERIVED_DATA_DIR)/Build/Products/$(CONFIGURATION)/$(PROJECT)

.PHONY: all build clean run findpath

all: build

build:
	xcodebuild -project $(PROJECT).xcodeproj -scheme $(PROJECT) -configuration $(CONFIGURATION)

clean:
	xcodebuild -project $(PROJECT).xcodeproj -scheme $(PROJECT) clean

# Print all potential paths where the executable might exist
findpath:
	@echo "Searching for executable..."
	@echo "DerivedData directory: $(DERIVED_DATA_DIR)"
	@echo "Expected executable path: $(EXECUTABLE_PATH)"
	@echo "Checking if executable exists: $(shell test -f $(EXECUTABLE_PATH) && echo "YES" || echo "NO")"
	@echo "Listing potential locations:"
	@find $(DERIVED_DATA_DIR)/Build/Products -type f -name "$(PROJECT)" 2>/dev/null || echo "No executables found"

run: build
	@echo "Attempting to run executable..."
	@if [ -f "$(EXECUTABLE_PATH)" ]; then \
		echo "Found executable at: $(EXECUTABLE_PATH)"; \
		$(EXECUTABLE_PATH); \
	else \
		echo "Executable not found in expected location. Searching..."; \
		FOUND_PATH=$$(find $(DERIVED_DATA_DIR)/Build/Products -type f -name "$(PROJECT)" | head -n 1); \
		if [ -n "$$FOUND_PATH" ]; then \
			echo "Found executable at: $$FOUND_PATH"; \
			$$FOUND_PATH; \
		else \
			echo "Error: Executable not found."; \
			echo "Try running 'make findpath' to debug."; \
			exit 1; \
		fi; \
	fi
