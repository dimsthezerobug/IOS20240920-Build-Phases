#!/bin/bash

# Extract the flavor from CONFIGURATION
# This regex pattern matches and captures everything after the last hyphen
# Examples:
#   Release-prod    -> FLAVOR = prod
#   Debug-dev       -> FLAVOR = dev
if [[ $CONFIGURATION =~ -([^-]*)$ ]]; then
    FLAVOR=${BASH_REMATCH[1]}
fi

# Check if FLAVOR is empty (in case the CONFIGURATION doesn't match the expected format)
if [ -z "$FLAVOR" ]; then
    # If no flavor could be extracted, use the default build
    # This could happen if the CONFIGURATION doesn't end with a hyphen followed by text
    # Example: "Release" (no flavor specified)
    echo "Warning: Could not extract flavor from CONFIGURATION. Using default build."
    "${FLUTTER_ROOT}/packages/flutter_tools/bin/xcode_backend.sh" build
else
    # If a flavor was successfully extracted, use it to set the target
    # This will look for a file named main_${FLAVOR}.dart in the lib directory
    # Examples:
    #   FLAVOR = prod -> target = lib/main_prod.dart
    #   FLAVOR = dev  -> target = lib/main_dev.dart
    #   FLAVOR = qa   -> target = lib/main_qa.dart
    echo "Building for flavor: $FLAVOR"
    "${FLUTTER_ROOT}/packages/flutter_tools/bin/xcode_backend.sh" build --target "lib/main_${FLAVOR}.dart"
fi

