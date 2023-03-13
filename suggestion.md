## Suggesting modifications by Takeshi Shimada

We are writing a book titled "Open-Ended Evolutionary Computation with Python ". In this book, we are using Evolution Gym. 
Evolution Gym is a great software, but we have found some issue with its installation on macOS. We would like to share them with you.

### 1. Cannot build because of type error.

 - An error occurred when I build (`pip install .` )
 - The line 271 of evogym/simulator/SimulatorCPP/Environment.cpp:
    There is a discrepancy in the type declaration in 
`Eigen::Ref<Eigen::Matrix<double, -1, -1, 0>, 0, Eigen::OuterStride<-1>>` 
 - However, the next code solve the problem.

```
    <https://github.com/TakesxiSximada/evogym/commit/58c26593cf511fe3d8d4606b20180a8ddc6396b0>
```
 - See the "Type Error" section for details.


### 2. Cannot build with specific version of CMake.

 - The latest version of CMake is 3.24.3, but the build fails with this version.
 - FindGLFW.cmake packaged with CMake seems to be the cause of the problem, but I'm not sure yet. It works when FindGLFW.cmake is removed.
 - I've tried to build with some versions of CMake:

| cmake version | Original source | Patched source |
|------------- |--------------- |-------------- |
| 3.22.2    | x        | o       |
| 3.24.2    | x        | o       |
| 3.25.1    | x        | x       |
| 3.25.3    | x        | x       |

 - See the "CMake Error" section for details.


(\*) "Patched source" means `<https://github.com/TakesxiSximada/evogym/commit/58c26593cf511fe3d8d4606b20180a8ddc6396b0>` has been applied.

### 3. My environment


| label | version etc             |
|------ |------------------------------------ |
| OS   | macOS Monterey 12.5.1        |
| CPU  | 2.3 GHz Dual Core Intel Core i5   |
| Memory | 16 GB 2133 MHz LPDDR3        |
| GPU  | Intel Iris Plus Graphics 640 1536 MB |

### 4. Details of Errors

#### Type Error

```
Processing /opt/ng/evogym
  Installing build dependencies ... done
  Getting requirements to build wheel ... done
  Preparing metadata (pyproject.toml) ... done
Building wheels for collected packages: evogym
  Building wheel for evogym (pyproject.toml) ... error
  error: subprocess-exited-with-error
  
  × Building wheel for evogym (pyproject.toml) did not run successfully.
  │ exit code: 1
  ?─> [212 lines of output]
      running bdist_wheel
      running build
      running build_py
      running build_ext
      -- pybind11 v2.9.0
      -- Including Cocoa support
      -- Configuring done
      -- Generating done
      -- Build files have been written to: /opt/ng/evogym/build/temp.macosx-12-x86_64-cpython-38
      Consolidate compiler generated dependencies of target glew
      [  2%] Built target docs
      [  7%] Built target glew
      Consolidate compiler generated dependencies of target glfw
      [ 62%] Built target glfw
      Consolidate compiler generated dependencies of target simulator_cpp
      [ 65%] Building CXX object SimulatorCPP/CMakeFiles/simulator_cpp.dir/PythonBindings.cpp.o
      [ 67%] Building CXX object SimulatorCPP/CMakeFiles/simulator_cpp.dir/Interface.cpp.o
      [ 70%] Building CXX object SimulatorCPP/CMakeFiles/simulator_cpp.dir/Environment.cpp.o
      [ 72%] Building CXX object SimulatorCPP/CMakeFiles/simulator_cpp.dir/ObjectCreator.cpp.o
      [ 75%] Building CXX object SimulatorCPP/CMakeFiles/simulator_cpp.dir/Sim.cpp.o
      [ 77%] Building CXX object SimulatorCPP/CMakeFiles/simulator_cpp.dir/main.cpp.o
      /opt/ng/evogym/evogym/simulator/SimulatorCPP/main.cpp:1:18: warning: extra tokens at end of #include directive [-Wextra-tokens]
      #include "main.h";
                       ^
                       //
      In file included from /opt/ng/evogym/evogym/simulator/SimulatorCPP/ObjectCreator.cpp:1:
      /opt/ng/evogym/evogym/simulator/SimulatorCPP/ObjectCreator.h:9:23: warning: extra tokens at end of #include directive [-Wextra-tokens]
      #include "SimObject.h";
                            ^
                            //
      In file included from /opt/ng/evogym/evogym/simulator/SimulatorCPP/Interface.cpp:1:      In file included from /opt/ng/evogym/evogym/simulator/SimulatorCPP/Interface.h:13:
      In file included from /opt/ng/evogym/evogym/simulator/SimulatorCPP/Sim.h:9:
      /opt/ng/evogym/evogym/simulator/SimulatorCPP/ObjectCreator.h:9:23: warning: extra tokens at end of #include directive [-Wextra-tokens]
      #include "SimObject.h";
                            ^
                            //
      In file included from /opt/ng/evogym/evogym/simulator/SimulatorCPP/PythonBindings.cpp:3:
      In file included from /opt/ng/evogym/evogym/simulator/SimulatorCPP/Interface.h:13:
      In file included from /opt/ng/evogym/evogym/simulator/SimulatorCPP/Sim.h:9:
      /opt/ng/evogym/evogym/simulator/SimulatorCPP/ObjectCreator.h:9:23: warning: extra tokens at end of #include directive [-Wextra-tokens]
      #include "SimObject.h";
                            ^
                            //
      In file included from /opt/ng/evogym/evogym/simulator/SimulatorCPP/main.cpp:11:
      In file included from /opt/ng/evogym/evogym/simulator/SimulatorCPP/Sim.h:9:
      /opt/ng/evogym/evogym/simulator/SimulatorCPP/ObjectCreator.h:9:23: warning: extra tokens at end of #include directive [-Wextra-tokens]
      #include "SimObject.h";
                            ^
                            //
      In file included from /opt/ng/evogym/evogym/simulator/SimulatorCPP/Sim.cpp:1:
      In file included from /opt/ng/evogym/evogym/simulator/SimulatorCPP/Sim.h:9:
      /opt/ng/evogym/evogym/simulator/SimulatorCPP/ObjectCreator.h:9:23: warning: extra tokens at end of #include directive [-Wextra-tokens]
      #include "SimObject.h";
                            ^
                            //
      /opt/ng/evogym/evogym/simulator/SimulatorCPP/Sim.cpp:19:28: warning: assigning field to itself [-Wself-assign-field]
              Sim::is_rendering_enabled = is_rendering_enabled;
                                        ^
      /opt/ng/evogym/evogym/simulator/SimulatorCPP/ObjectCreator.cpp:334:49: warning: comparison between NULL and non-pointer ('int' and NULL) [-Wnull-arithmetic]
                              if (top != NULL && top->point_bot_left_index != NULL)
                                                 ~~~~~~~~~~~~~~~~~~~~~~~~~ ^  ~~~~
      /opt/ng/evogym/evogym/simulator/SimulatorCPP/ObjectCreator.cpp:336:52: warning: comparison between NULL and non-pointer ('int' and NULL) [-Wnull-arithmetic]
                              if (left != NULL && left->point_top_right_index != NULL)
                                                  ~~~~~~~~~~~~~~~~~~~~~~~~~~~ ^  ~~~~
      /opt/ng/evogym/evogym/simulator/SimulatorCPP/ObjectCreator.cpp:338:60: warning: comparison between NULL and non-pointer ('int' and NULL) [-Wnull-arithmetic]
                              if (top_left != NULL && top_left->point_bot_right_index != NULL)
                                                      ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ ^  ~~~~
      /opt/ng/evogym/evogym/simulator/SimulatorCPP/ObjectCreator.cpp:340:38: warning: comparison between NULL and non-pointer ('int' and NULL) [-Wnull-arithmetic]
                              if (current->point_top_left_index == NULL)
                                  ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ ^  ~~~~
      /opt/ng/evogym/evogym/simulator/SimulatorCPP/ObjectCreator.cpp:349:50: warning: comparison between NULL and non-pointer ('int' and NULL) [-Wnull-arithmetic]
                              if (top != NULL && top->point_bot_right_index != NULL)
                                                 ~~~~~~~~~~~~~~~~~~~~~~~~~~ ^  ~~~~
      /opt/ng/evogym/evogym/simulator/SimulatorCPP/ObjectCreator.cpp:351:53: warning: comparison between NULL and non-pointer ('int' and NULL) [-Wnull-arithmetic]
                              if (right != NULL && right->point_top_left_index != NULL)
                                                   ~~~~~~~~~~~~~~~~~~~~~~~~~~~ ^  ~~~~
      /opt/ng/evogym/evogym/simulator/SimulatorCPP/ObjectCreator.cpp:353:61: warning: comparison between NULL and non-pointer ('int' and NULL) [-Wnull-arithmetic]
                              if (top_right != NULL && top_right->point_bot_left_index != NULL)
                                                       ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ ^  ~~~~
      /opt/ng/evogym/evogym/simulator/SimulatorCPP/ObjectCreator.cpp:355:39: warning: comparison between NULL and non-pointer ('int' and NULL) [-Wnull-arithmetic]
                              if (current->point_top_right_index == NULL)
                                  ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ ^  ~~~~
      /opt/ng/evogym/evogym/simulator/SimulatorCPP/ObjectCreator.cpp:364:49: warning: comparison between NULL and non-pointer ('int' and NULL) [-Wnull-arithmetic]
                              if (bot != NULL && bot->point_top_left_index != NULL)
                                                 ~~~~~~~~~~~~~~~~~~~~~~~~~ ^  ~~~~
      /opt/ng/evogym/evogym/simulator/SimulatorCPP/ObjectCreator.cpp:366:52: warning: comparison between NULL and non-pointer ('int' and NULL) [-Wnull-arithmetic]
                              if (left != NULL && left->point_bot_right_index != NULL)
                                                  ~~~~~~~~~~~~~~~~~~~~~~~~~~~ ^  ~~~~
      /opt/ng/evogym/evogym/simulator/SimulatorCPP/ObjectCreator.cpp:368:60: warning: comparison between NULL and non-pointer ('int' and NULL) [-Wnull-arithmetic]
                              if (bot_left != NULL && bot_left->point_top_right_index != NULL)
                                                      ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ ^  ~~~~
      /opt/ng/evogym/evogym/simulator/SimulatorCPP/ObjectCreator.cpp:370:38: warning: comparison between NULL and non-pointer ('int' and NULL) [-Wnull-arithmetic]
                              if (current->point_bot_left_index == NULL)
                                  ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ ^  ~~~~
      /opt/ng/evogym/evogym/simulator/SimulatorCPP/ObjectCreator.cpp:380:50: warning: comparison between NULL and non-pointer ('int' and NULL) [-Wnull-arithmetic]
                              if (bot != NULL && bot->point_top_right_index != NULL)
                                                 ~~~~~~~~~~~~~~~~~~~~~~~~~~ ^  ~~~~
      /opt/ng/evogym/evogym/simulator/SimulatorCPP/ObjectCreator.cpp:382:53: warning: comparison between NULL and non-pointer ('int' and NULL) [-Wnull-arithmetic]
                              if (right != NULL && right->point_bot_left_index != NULL)
                                                   ~~~~~~~~~~~~~~~~~~~~~~~~~~~ ^  ~~~~
      /opt/ng/evogym/evogym/simulator/SimulatorCPP/ObjectCreator.cpp:384:61: warning: comparison between NULL and non-pointer ('int' and NULL) [-Wnull-arithmetic]
                              if (bot_right != NULL && bot_right->point_top_left_index != NULL)
                                                       ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ ^  ~~~~
      /opt/ng/evogym/evogym/simulator/SimulatorCPP/ObjectCreator.cpp:386:39: warning: comparison between NULL and non-pointer ('int' and NULL) [-Wnull-arithmetic]
                              if (current->point_bot_right_index == NULL)
                                  ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ ^  ~~~~
      /opt/ng/evogym/evogym/simulator/SimulatorCPP/ObjectCreator.cpp:398:43: warning: comparison between NULL and non-pointer ('int' and NULL) [-Wnull-arithmetic]
                              if (top != NULL && top->edge_bot_index != NULL)
                                                 ~~~~~~~~~~~~~~~~~~~ ^  ~~~~
      /opt/ng/evogym/evogym/simulator/SimulatorCPP/ObjectCreator.cpp:400:32: warning: comparison between NULL and non-pointer ('int' and NULL) [-Wnull-arithmetic]
                              if (current->edge_top_index == NULL) {
                                  ~~~~~~~~~~~~~~~~~~~~~~~ ^  ~~~~
      /opt/ng/evogym/evogym/simulator/SimulatorCPP/ObjectCreator.cpp:405:43: warning: comparison between NULL and non-pointer ('int' and NULL) [-Wnull-arithmetic]
                              if (bot != NULL && bot->edge_top_index != NULL)
                                                 ~~~~~~~~~~~~~~~~~~~ ^  ~~~~
      /opt/ng/evogym/evogym/simulator/SimulatorCPP/ObjectCreator.cpp:407:32: warning: comparison between NULL and non-pointer ('int' and NULL) [-Wnull-arithmetic]
                              if (current->edge_bot_index == NULL) {
                                  ~~~~~~~~~~~~~~~~~~~~~~~ ^  ~~~~
      /opt/ng/evogym/evogym/simulator/SimulatorCPP/ObjectCreator.cpp:412:47: warning: comparison between NULL and non-pointer ('int' and NULL) [-Wnull-arithmetic]
                              if (left != NULL && left->edge_right_index != NULL)
                                                  ~~~~~~~~~~~~~~~~~~~~~~ ^  ~~~~
      /opt/ng/evogym/evogym/simulator/SimulatorCPP/ObjectCreator.cpp:414:33: warning: comparison between NULL and non-pointer ('int' and NULL) [-Wnull-arithmetic]
                              if (current->edge_left_index == NULL) {
                                  ~~~~~~~~~~~~~~~~~~~~~~~~ ^  ~~~~
      /opt/ng/evogym/evogym/simulator/SimulatorCPP/ObjectCreator.cpp:419:48: warning: comparison between NULL and non-pointer ('int' and NULL) [-Wnull-arithmetic]
                              if (right != NULL && right->edge_left_index != NULL)
                                                   ~~~~~~~~~~~~~~~~~~~~~~ ^  ~~~~
      /opt/ng/evogym/evogym/simulator/SimulatorCPP/ObjectCreator.cpp:421:34: warning: comparison between NULL and non-pointer ('int' and NULL) [-Wnull-arithmetic]
                              if (current->edge_right_index == NULL) {
                                  ~~~~~~~~~~~~~~~~~~~~~~~~~ ^  ~~~~
      /opt/ng/evogym/evogym/simulator/SimulatorCPP/Environment.cpp:271:10: error: calling a private constructor of class 'Eigen::Ref<Eigen::Matrix<double, -1, -1, 0>, 0, Eigen::OuterStride<-1>>'
                      return empty;
                             ^
      /opt/ng/evogym/evogym/simulator/externals/eigen/Eigen/src/Core/Ref.h:299:30: note: declared private here
          EIGEN_DEVICE_FUNC inline Ref(const PlainObjectBase<Derived>& expr,
                                   ^
      /opt/ng/evogym/evogym/simulator/SimulatorCPP/Environment.cpp:282:10: error: calling a private constructor of class 'Eigen::Ref<Eigen::Matrix<double, -1, -1, 0>, 0, Eigen::OuterStride<-1>>'
                      return empty;
                             ^
      /opt/ng/evogym/evogym/simulator/externals/eigen/Eigen/src/Core/Ref.h:299:30: note: declared private here
          EIGEN_DEVICE_FUNC inline Ref(const PlainObjectBase<Derived>& expr,
                                   ^
      /opt/ng/evogym/evogym/simulator/SimulatorCPP/Environment.cpp:294:10: error: calling a private constructor of class 'Eigen::Ref<Eigen::Matrix<double, -1, -1, 0>, 0, Eigen::OuterStride<-1>>'
                      return empty;
                             ^
      /opt/ng/evogym/evogym/simulator/externals/eigen/Eigen/src/Core/Ref.h:299:30: note: declared private here
          EIGEN_DEVICE_FUNC inline Ref(const PlainObjectBase<Derived>& expr,
                                   ^
      /opt/ng/evogym/evogym/simulator/SimulatorCPP/Environment.cpp:309:10: error: calling a private constructor of class 'Eigen::Ref<Eigen::Matrix<double, -1, -1, 0>, 0, Eigen::OuterStride<-1>>'
                      return empty;
                             ^
      /opt/ng/evogym/evogym/simulator/externals/eigen/Eigen/src/Core/Ref.h:299:30: note: declared private here
          EIGEN_DEVICE_FUNC inline Ref(const PlainObjectBase<Derived>& expr,
                                   ^
      1 warning generated.
      2 warnings generated.
      4 errors generated.
      make[2]: *** [SimulatorCPP/CMakeFiles/simulator_cpp.dir/Environment.cpp.o] Error 1
      make[2]: *** Waiting for unfinished jobs....
      25 warnings generated.
      2 warnings generated.
      1 warning generated.
      make[1]: *** [SimulatorCPP/CMakeFiles/simulator_cpp.dir/all] Error 2
      make: *** [all] Error 2
      Traceback (most recent call last):
        File "/Users/sximada/.venv/cmake-3.24.1/lib/python3.8/site-packages/pip/_vendor/pep517/in_process/_in_process.py", line 363, in <module>
          main()
        File "/Users/sximada/.venv/cmake-3.24.1/lib/python3.8/site-packages/pip/_vendor/pep517/in_process/_in_process.py", line 345, in main
          json_out['return_val'] = hook(**hook_input['kwargs'])
        File "/Users/sximada/.venv/cmake-3.24.1/lib/python3.8/site-packages/pip/_vendor/pep517/in_process/_in_process.py", line 261, in build_wheel
          return _build_backend().build_wheel(wheel_directory, config_settings,
        File "/private/var/folders/0m/47l1rtnx3s9dzk560gpfykh00000gp/T/pip-build-env-whl0lls9/overlay/lib/python3.8/site-packages/setuptools/build_meta.py", line 413, in build_wheel
          return self._build_with_temp_dir(['bdist_wheel'], '.whl',
        File "/private/var/folders/0m/47l1rtnx3s9dzk560gpfykh00000gp/T/pip-build-env-whl0lls9/overlay/lib/python3.8/site-packages/setuptools/build_meta.py", line 398, in _build_with_temp_dir
          self.run_setup()
        File "/private/var/folders/0m/47l1rtnx3s9dzk560gpfykh00000gp/T/pip-build-env-whl0lls9/overlay/lib/python3.8/site-packages/setuptools/build_meta.py", line 335, in run_setup
          exec(code, locals())
        File "<string>", line 61, in <module>
        File "/private/var/folders/0m/47l1rtnx3s9dzk560gpfykh00000gp/T/pip-build-env-whl0lls9/overlay/lib/python3.8/site-packages/setuptools/__init__.py", line 108, in setup
          return distutils.core.setup(**attrs)
        File "/private/var/folders/0m/47l1rtnx3s9dzk560gpfykh00000gp/T/pip-build-env-whl0lls9/overlay/lib/python3.8/site-packages/setuptools/_distutils/core.py", line 185, in setup
          return run_commands(dist)
        File "/private/var/folders/0m/47l1rtnx3s9dzk560gpfykh00000gp/T/pip-build-env-whl0lls9/overlay/lib/python3.8/site-packages/setuptools/_distutils/core.py", line 201, in run_commands
          dist.run_commands()
        File "/private/var/folders/0m/47l1rtnx3s9dzk560gpfykh00000gp/T/pip-build-env-whl0lls9/overlay/lib/python3.8/site-packages/setuptools/_distutils/dist.py", line 969, in run_commands
          self.run_command(cmd)
        File "/private/var/folders/0m/47l1rtnx3s9dzk560gpfykh00000gp/T/pip-build-env-whl0lls9/overlay/lib/python3.8/site-packages/setuptools/dist.py", line 1221, in run_command
          super().run_command(command)
        File "/private/var/folders/0m/47l1rtnx3s9dzk560gpfykh00000gp/T/pip-build-env-whl0lls9/overlay/lib/python3.8/site-packages/setuptools/_distutils/dist.py", line 988, in run_command
          cmd_obj.run()
        File "/private/var/folders/0m/47l1rtnx3s9dzk560gpfykh00000gp/T/pip-build-env-whl0lls9/overlay/lib/python3.8/site-packages/wheel/bdist_wheel.py", line 325, in run
          self.run_command("build")
        File "/private/var/folders/0m/47l1rtnx3s9dzk560gpfykh00000gp/T/pip-build-env-whl0lls9/overlay/lib/python3.8/site-packages/setuptools/_distutils/cmd.py", line 318, in run_command
          self.distribution.run_command(command)
        File "/private/var/folders/0m/47l1rtnx3s9dzk560gpfykh00000gp/T/pip-build-env-whl0lls9/overlay/lib/python3.8/site-packages/setuptools/dist.py", line 1221, in run_command
          super().run_command(command)
        File "/private/var/folders/0m/47l1rtnx3s9dzk560gpfykh00000gp/T/pip-build-env-whl0lls9/overlay/lib/python3.8/site-packages/setuptools/_distutils/dist.py", line 988, in run_command
          cmd_obj.run()
        File "/private/var/folders/0m/47l1rtnx3s9dzk560gpfykh00000gp/T/pip-build-env-whl0lls9/overlay/lib/python3.8/site-packages/setuptools/_distutils/command/build.py", line 131, in run
          self.run_command(cmd_name)
        File "/private/var/folders/0m/47l1rtnx3s9dzk560gpfykh00000gp/T/pip-build-env-whl0lls9/overlay/lib/python3.8/site-packages/setuptools/_distutils/cmd.py", line 318, in run_command
          self.distribution.run_command(command)
        File "/private/var/folders/0m/47l1rtnx3s9dzk560gpfykh00000gp/T/pip-build-env-whl0lls9/overlay/lib/python3.8/site-packages/setuptools/dist.py", line 1221, in run_command
          super().run_command(command)
        File "/private/var/folders/0m/47l1rtnx3s9dzk560gpfykh00000gp/T/pip-build-env-whl0lls9/overlay/lib/python3.8/site-packages/setuptools/_distutils/dist.py", line 988, in run_command
          cmd_obj.run()
        File "<string>", line 32, in run
        File "<string>", line 58, in build_extension
        File "/usr/local/Cellar/python@3.8/3.8.16/Frameworks/Python.framework/Versions/3.8/lib/python3.8/subprocess.py", line 364, in check_call
          raise CalledProcessError(retcode, cmd)
      subprocess.CalledProcessError: Command '['cmake', '--build', '.', '--config', 'Release', '--', '-j8']' returned non-zero exit status 2.
      [end of output]
  
  note: This error originates from a subprocess, and is likely not a problem with pip.
  ERROR: Failed building wheel for evogym
Failed to build evogym
ERROR: Could not build wheels for evogym, which is required to install pyproject.toml-based projects
```


#### CMake Error

```
Processing /opt/ng/evogym
  Installing build dependencies ... done
  Getting requirements to build wheel ... done
  Preparing metadata (pyproject.toml) ... done
Building wheels for collected packages: evogym
  Building wheel for evogym (pyproject.toml) ... error
  error: subprocess-exited-with-error
  
  × Building wheel for evogym (pyproject.toml) did not run successfully.
  │ exit code: 1
  ?─> [98 lines of output]
      running bdist_wheel
      running build
      running build_py
      running build_ext
      -- The C compiler identification is AppleClang 13.1.6.13160021
      -- The CXX compiler identification is AppleClang 13.1.6.13160021
      -- Detecting C compiler ABI info
      -- Detecting C compiler ABI info - done
      -- Check for working C compiler: /Library/Developer/CommandLineTools/usr/bin/cc - skipped
      -- Detecting C compile features
      -- Detecting C compile features - done
      -- Detecting CXX compiler ABI info
      -- Detecting CXX compiler ABI info - done
      -- Check for working CXX compiler: /usr/bin/g++ - skipped
      -- Detecting CXX compile features
      -- Detecting CXX compile features - done
      CMake Error at /usr/local/Cellar/cmake/3.25.3/share/cmake/Modules/FindGLEW.cmake:72 (get_target_property):
        get_target_property() called with non-existent target "GLEW::GLEW".
      Call Stack (most recent call first):
        CMakeLists.txt:22 (find_package)
      
      
      CMake Error at /usr/local/Cellar/cmake/3.25.3/share/cmake/Modules/FindGLEW.cmake:74 (get_target_property):
        get_target_property() called with non-existent target "GLEW::GLEW".
      Call Stack (most recent call first):
        CMakeLists.txt:22 (find_package)
      
      
      CMake Error at /usr/local/Cellar/cmake/3.25.3/share/cmake/Modules/FindGLEW.cmake:79 (get_target_property):
        get_target_property() called with non-existent target "GLEW::GLEW".
      Call Stack (most recent call first):
        CMakeLists.txt:22 (find_package)
      
      
      CMake Error at /usr/local/Cellar/cmake/3.25.3/share/cmake/Modules/FindGLEW.cmake:80 (get_target_property):
        get_target_property() called with non-existent target "GLEW::GLEW".
      Call Stack (most recent call first):
        CMakeLists.txt:22 (find_package)
      
      
      CMake Error at /usr/local/Cellar/cmake/3.25.3/share/cmake/Modules/FindGLEW.cmake:82 (get_target_property):
        get_target_property() called with non-existent target "GLEW::GLEW".
      Call Stack (most recent call first):
        CMakeLists.txt:22 (find_package)
      
      
      -- pybind11 v2.9.0
      -- Including Cocoa support
      -- Configuring incomplete, errors occurred!
      See also "/opt/ng/evogym/build/temp.macosx-12-x86_64-cpython-38/CMakeFiles/CMakeOutput.log".
      See also "/opt/ng/evogym/build/temp.macosx-12-x86_64-cpython-38/CMakeFiles/CMakeError.log".
      Traceback (most recent call last):
        File "/Users/sximada/.venv/cmake-3.24.1/lib/python3.8/site-packages/pip/_vendor/pep517/in_process/_in_process.py", line 363, in <module>
          main()
        File "/Users/sximada/.venv/cmake-3.24.1/lib/python3.8/site-packages/pip/_vendor/pep517/in_process/_in_process.py", line 345, in main
          json_out['return_val'] = hook(**hook_input['kwargs'])
        File "/Users/sximada/.venv/cmake-3.24.1/lib/python3.8/site-packages/pip/_vendor/pep517/in_process/_in_process.py", line 261, in build_wheel
          return _build_backend().build_wheel(wheel_directory, config_settings,
        File "/private/var/folders/0m/47l1rtnx3s9dzk560gpfykh00000gp/T/pip-build-env-pm1tm6c6/overlay/lib/python3.8/site-packages/setuptools/build_meta.py", line 413, in build_wheel
          return self._build_with_temp_dir(['bdist_wheel'], '.whl',
        File "/private/var/folders/0m/47l1rtnx3s9dzk560gpfykh00000gp/T/pip-build-env-pm1tm6c6/overlay/lib/python3.8/site-packages/setuptools/build_meta.py", line 398, in _build_with_temp_dir
          self.run_setup()
        File "/private/var/folders/0m/47l1rtnx3s9dzk560gpfykh00000gp/T/pip-build-env-pm1tm6c6/overlay/lib/python3.8/site-packages/setuptools/build_meta.py", line 335, in run_setup
          exec(code, locals())
        File "<string>", line 62, in <module>
        File "/private/var/folders/0m/47l1rtnx3s9dzk560gpfykh00000gp/T/pip-build-env-pm1tm6c6/overlay/lib/python3.8/site-packages/setuptools/__init__.py", line 108, in setup
          return distutils.core.setup(**attrs)
        File "/private/var/folders/0m/47l1rtnx3s9dzk560gpfykh00000gp/T/pip-build-env-pm1tm6c6/overlay/lib/python3.8/site-packages/setuptools/_distutils/core.py", line 185, in setup
          return run_commands(dist)
        File "/private/var/folders/0m/47l1rtnx3s9dzk560gpfykh00000gp/T/pip-build-env-pm1tm6c6/overlay/lib/python3.8/site-packages/setuptools/_distutils/core.py", line 201, in run_commands
          dist.run_commands()
        File "/private/var/folders/0m/47l1rtnx3s9dzk560gpfykh00000gp/T/pip-build-env-pm1tm6c6/overlay/lib/python3.8/site-packages/setuptools/_distutils/dist.py", line 969, in run_commands
          self.run_command(cmd)
        File "/private/var/folders/0m/47l1rtnx3s9dzk560gpfykh00000gp/T/pip-build-env-pm1tm6c6/overlay/lib/python3.8/site-packages/setuptools/dist.py", line 1221, in run_command          super().run_command(command)
        File "/private/var/folders/0m/47l1rtnx3s9dzk560gpfykh00000gp/T/pip-build-env-pm1tm6c6/overlay/lib/python3.8/site-packages/setuptools/_distutils/dist.py", line 988, in run_command
          cmd_obj.run()
        File "/private/var/folders/0m/47l1rtnx3s9dzk560gpfykh00000gp/T/pip-build-env-pm1tm6c6/overlay/lib/python3.8/site-packages/wheel/bdist_wheel.py", line 325, in run
          self.run_command("build")
        File "/private/var/folders/0m/47l1rtnx3s9dzk560gpfykh00000gp/T/pip-build-env-pm1tm6c6/overlay/lib/python3.8/site-packages/setuptools/_distutils/cmd.py", line 318, in run_command
          self.distribution.run_command(command)
        File "/private/var/folders/0m/47l1rtnx3s9dzk560gpfykh00000gp/T/pip-build-env-pm1tm6c6/overlay/lib/python3.8/site-packages/setuptools/dist.py", line 1221, in run_command          super().run_command(command)
        File "/private/var/folders/0m/47l1rtnx3s9dzk560gpfykh00000gp/T/pip-build-env-pm1tm6c6/overlay/lib/python3.8/site-packages/setuptools/_distutils/dist.py", line 988, in run_command
          cmd_obj.run()
        File "/private/var/folders/0m/47l1rtnx3s9dzk560gpfykh00000gp/T/pip-build-env-pm1tm6c6/overlay/lib/python3.8/site-packages/setuptools/_distutils/command/build.py", line 131, in run
          self.run_command(cmd_name)
        File "/private/var/folders/0m/47l1rtnx3s9dzk560gpfykh00000gp/T/pip-build-env-pm1tm6c6/overlay/lib/python3.8/site-packages/setuptools/_distutils/cmd.py", line 318, in run_command
          self.distribution.run_command(command)
        File "/private/var/folders/0m/47l1rtnx3s9dzk560gpfykh00000gp/T/pip-build-env-pm1tm6c6/overlay/lib/python3.8/site-packages/setuptools/dist.py", line 1221, in run_command          super().run_command(command)
        File "/private/var/folders/0m/47l1rtnx3s9dzk560gpfykh00000gp/T/pip-build-env-pm1tm6c6/overlay/lib/python3.8/site-packages/setuptools/_distutils/dist.py", line 988, in run_command
          cmd_obj.run()
        File "<string>", line 32, in run
        File "<string>", line 58, in build_extension
        File "/usr/local/Cellar/python@3.8/3.8.16/Frameworks/Python.framework/Versions/3.8/lib/python3.8/subprocess.py", line 364, in check_call
          raise CalledProcessError(retcode, cmd)
      subprocess.CalledProcessError: Command '['cmake', '/opt/ng/evogym/evogym/simulator', '-DCMAKE_CXX_COMPILER=/usr/bin/g++', '-DCMAKE_LIBRARY_OUTPUT_DIRECTORY=/opt/ng/evogym/build/lib.macosx-12-x86_64-cpython-38/evogym', '-DPYTHON_EXECUTABLE=/Users/sximada/.venv/cmake-3.24.1/bin/python3.8', '-DCMAKE_BUILD_TYPE=Release']' returned non-zero exit status 1.
      [end of output]
  
  note: This error originates from a subprocess, and is likely not a problem with pip.
  ERROR: Failed building wheel for evogym
Failed to build evogym
ERROR: Could not build wheels for evogym, which is required to install pyproject.toml-based projects
```

