cmake_minimum_required(VERSION 3.22)
project(BasicEQ VERSION 1.0.0 LANGUAGES C CXX)

# 1. Force C++20 for modern JUCE development
set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# 2. Tell CMake to download JUCE automatically so you don't have to manage it
include(FetchContent)
FetchContent_Declare(
    JUCE
    GIT_REPOSITORY https://github.com/juce-framework/JUCE.git
    GIT_TAG master # Automatically fetches the latest stable JUCE 8 release
    GIT_SHALLOW TRUE
)
FetchContent_MakeAvailable(JUCE)

# 3. Create the Audio Effect Plugin target
juce_add_plugin(BasicEQ
    COMPANY_NAME "MyCompany"
    IS_SYNTH FALSE                       # FALSE = Audio Effect (VST3/Standalone)
    NEEDS_MIDI_INPUT FALSE               
    NEEDS_MIDI_OUTPUT FALSE
    PLUGIN_MANUFACTURER_CODE Mvco        # Exactly 4 characters
    PLUGIN_CODE Efx1                     # Exactly 4 characters
    FORMATS VST3 Standalone              # Standalone exe + VST3 bundle
    PRODUCT_NAME "Basic EQ")

# 4. Point to your source files (ensure these exist in a "Source" folder)
target_sources(BasicEQ PRIVATE
    Source/PluginProcessor.cpp
    Source/PluginProcessor.h
    Source/PluginEditor.cpp
    Source/PluginEditor.h
)

# 5. Link standard JUCE components
target_link_libraries(BasicEQ PRIVATE
    juce::juce_audio_utils
    juce::juce_audio_processors
    juce::juce_audio_formats
    juce::juce_audio_basics
    juce::juce_gui_extra
    juce::juce_graphics
    juce::juce_data_structures
    juce::juce_events
    juce::juce_core
)

# 6. Windows-specific compiler optimization for MSVC
if(MSVC)
    target_compile_options(BasicEQ PRIVATE /MP /utf-8)
endif()
