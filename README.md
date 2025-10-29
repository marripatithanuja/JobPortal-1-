@echo off
REM Improved run_backend.bat for Windows

REM Check if lib folder exists
if not exist lib (
    echo ERROR: lib folder not found. Please create 'lib' and add all required JARs.
    pause
    exit /b
)

REM Create output directory if not exists
if not exist out mkdir out

REM Find all .java files and compile them
setlocal enabledelayedexpansion
set SOURCES=
for /R src\main\java %%f in (*.java) do set SOURCES=!SOURCES! %%f

REM Compile all Java files
javac -cp "lib/*" -d out !SOURCES!
if errorlevel 1 (
    echo ERROR: Compilation failed. Check for missing JARs or code errors.
    pause
    exit /b
)

REM Run the application
java -cp "out;lib/*;src/main/resources" com.example.jobboard.JobBoardApplication
if errorlevel 1 (
    echo ERROR: Application failed to start. Check for runtime errors or missing JARs.
)

pause
