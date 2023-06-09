set(SOURCES AppParamParserLinux.cpp
            CPUInfoLinux.cpp
            MemUtils.cpp
            OptionalsReg.cpp
            PlatformLinux.cpp
            SysfsPath.cpp
            TimeUtils.cpp)

set(HEADERS AppParamParserLinux.h
            CPUInfoLinux.h
            OptionalsReg.h
            PlatformLinux.h
            SysfsPath.h
            TimeUtils.h)

if(ALSA_FOUND)
  list(APPEND SOURCES FDEventMonitor.cpp)
  list(APPEND HEADERS FDEventMonitor.h)
endif()

if(DBUS_FOUND)
  list(APPEND SOURCES DBusMessage.cpp
                      DBusUtil.cpp)
  list(APPEND HEADERS DBusMessage.h
                      DBusUtil.h)
endif()

if(ADDONS_CONFIGURE_AT_STARTUP)
  add_compile_definitions(ADDONS_CONFIGURE_AT_STARTUP)
endif()

if("aml" IN_LIST CORE_PLATFORM_NAME_LC)
  list(APPEND SOURCES ScreenshotSurfaceAML.cpp)
  list(APPEND HEADERS ScreenshotSurfaceAML.h)
endif()

core_add_library(linuxsupport)
