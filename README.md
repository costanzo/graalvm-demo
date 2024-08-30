#

## Configure Graal

```shell
export JAVA_HOME=/path/to/graalvm-jdk-17.0.12+8.1
```

If the following error occurs, you need to configure the visual studio build tools.

```shell
[1/8] Initializing...
                                                                                    (0.0s @ 0.29GB)
Error: On Windows, GraalVM Native Image for JDK 17 requires Visual Studio 2022 version 17.1.0 or later (C/C++ Optimizing Compiler Version 19.31 or later).
Compiler info detected: cl.exe (microsoft, x64, 19.16.27051)
Error: To prevent native-toolchain checking provide command-line option -H:-CheckToolchain
Error: Use -H:+ReportExceptionStackTraces to print stacktrace of underlying exception
```

Find the `native-image.cmd` file in your GraalVM bin folder, and manually add the `call "E:\Microsoft Visual Studio\2022\BuildTools\VC\Auxiliary\Build\vcvars64.bat"` instruction.

```shell
@echo off

call "E:\Microsoft Visual Studio\2022\BuildTools\VC\Auxiliary\Build\vcvars64.bat"
call :getScriptLocation location
set "GRAALVM_ARGUMENT_VECTOR_PROGRAM_NAME=%~0"
"%location%..\lib\svm\bin\native-image.exe" %*
exit /b %errorlevel%

:: If this script is in `%PATH%` and called quoted without a full path (e.g., `"js"`), `%~dp0` is expanded to `cwd`
:: rather than the path to the script.
:: This does not happen if `%~dp0` is accessed in a subroutine.
:getScriptLocation variableName
    set "%~1=%~dp0"
    exit /b 0
```