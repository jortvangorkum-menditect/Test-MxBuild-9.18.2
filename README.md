# Test-MxBuild-9.18.2
This repository is for testing if the MxBuild 9.18.2 works with GitHub Actions

## Reproducable steps:
- Create a Mendix application
- Define a GitHub Actions Workflow which:
  - sets up java 17
  - download and unzip the mxbuild version 9.18.2.55931 in the folder "mxbuild-9.18.2" using the command:
```sh
curl -L -O https://cdn.mendix.com/runtime/mxbuild-9.18.2.55931.tar.gz
mkdir mxbuild-9.18.2
tar -xzf mxbuild-9.18.2.55931.tar.gz -C mxbuild-9.18.2
rm mxbuild-9.18.2.55931.tar.gz
```

  - execute the mxbuild with the command:
```sh
./mxbuilds/mxbuild-9.18.2/modeler/mxbuild.exe \
            --java-home=$JAVA_HOME \
            --java-exe-path=$JAVA_HOME/bin/java.exe \
            --target=package \
            --loose-version-check \
            --output=./out/Test-MxBuild-9.18.2.mda \
            "./project/Test MxBuild.mpr"
```

## File structure explanation

```powershell
root
├───.github
│   ├───actions
│   │   └───download-mxbuild # A helper for downloading MxBuild
│   └───workflows
|       └───build.yml # The build file for executing MxBuild on the Mendix project
├───logfiles
│   ├───MxBuild-LogLevel-Debug # The build log file from mxbuild with loglevel=Debug
│   └───MxBuild-LogLevel-Info # The build log file from mxbuild with loglevel=Info
├───mxbuilds # The location where the download MxBuild gets stored
├───out # Where the compiled MDA should be stored
└───project # The location of the Mendix Project
```

## The error and its entire stacktrace
```powershell
ERROR: System.ComponentModel.Win32Exception (0x80004005): The system cannot find the file specified
    at System.Diagnostics.Process.StartWithCreateProcess(ProcessStartInfo startInfo)
    at Mendix.Modeler.Utility.Processes.ProcessRunner.RunProcessAsynchronously(Process process, ProcessInfo processInfo) in C:\Users\Autobuild\workspace\AppStudio4.0-Build\modeler\Mendix.Modeler.Utility\Processes\ProcessRunner.cs:line 58
    at Mendix.Modeler.Utility.Processes.ProcessRunner.RunProcessSynchronously(ProcessInfo processInfo) in C:\Users\Autobuild\workspace\AppStudio4.0-Build\modeler\Mendix.Modeler.Utility\Processes\ProcessRunner.cs:line 44
    at Mendix.Modeler.WebUI.Export.Packaging.GruntRunner.RunGrunt(String buildFilePath, String deploymentDir) in C:\Users\Autobuild\workspace\AppStudio4.0-Build\modeler\Mendix.Modeler.WebUI.Export\Packaging\GruntRunner.cs:line 33
    at Mendix.Modeler.WebUI.Deployment.WebUIDeploymentWorker.ExportWidgets(IProject project, IProgressInfo info, DeploymentSettings settings) in C:\Users\Autobuild\workspace\AppStudio4.0-Build\modeler\Mendix.Modeler.WebUI\Deployment\WebUIDeploymentWorker.cs:line 269
    at Mendix.Modeler.WebUI.Deployment.WebUIDeploymentWorker.DoWork(DeploymentPhase phase, IProject project, DeploymentSettings settings, IProgressInfo info) in C:\Users\Autobuild\workspace\AppStudio4.0-Build\modeler\Mendix.Modeler.WebUI\Deployment\WebUIDeploymentWorker.cs:line 156
    at Mendix.Modeler.Deployment.DeploymentProcessBuilder.ExecuteDeploymentWorkForPhase(DeploymentPhase phase, IProject project, DeploymentSettings settings, IProgressInfo info) in C:\Users\Autobuild\workspace\AppStudio4.0-Build\modeler\Mendix.Modeler.Deployment\DeploymentProcessBuilder.cs:line 181
    at Mendix.Modeler.Deployment.DeploymentProcessBuilder.<>c__DisplayClass12_1.<AddBuildStepWithOverride>b__2() in C:\Users\Autobuild\workspace\AppStudio4.0-Build\modeler\Mendix.Modeler.Deployment\DeploymentProcessBuilder.cs:line 149
    at Mendix.Modeler.Projects.ProjectStructureManager.<>c__DisplayClass2_0.<DoWithOverride>b__0() in C:\Users\Autobuild\workspace\AppStudio4.0-Build\modeler\Mendix.Modeler.Core\Projects\ProjectStructureManager.cs:line 41
    at Mendix.Modeler.Projects.ProjectStructureManager.Do[T](IProject projectOverride, Func`1 action) in C:\Users\Autobuild\workspace\AppStudio4.0-Build\modeler\Mendix.Modeler.Core\Projects\ProjectStructureManager.cs:line 60
    at Mendix.Modeler.ProtectedModules.ProtectedModulesUnlocker.<>c__DisplayClass3_0.<Do>b__0() in C:\Users\Autobuild\workspace\AppStudio4.0-Build\modeler\Mendix.Modeler.Core\ProtectedModules\ProtectedModulesUnlocker.cs:line 23
    at Mendix.Modeler.ProtectedModules.ProtectedModulesUnlocker.ExecuteInContext[T](Boolean unlockImplementation, Func`1 code) in C:\Users\Autobuild\workspace\AppStudio4.0-Build\modeler\Mendix.Modeler.Core\ProtectedModules\ProtectedModulesUnlocker.cs:line 42
    at Mendix.CommandLine.Shared.Progress.ProcessRunner.Run() in C:\Users\Autobuild\workspace\AppStudio4.0-Build\modeler\Mendix.CommandLine.Shared\Progress\ProcessRunner.cs:line 41
    at Mendix.MxBuild.BuildRunner.BuildProject(IProject project, IBuildOptions options, List`1 allowedLocations) in C:\Users\Autobuild\workspace\AppStudio4.0-Build\modeler\Mendix.MxBuild\Build\BuildRunner.cs:line 156
    at Mendix.MxBuild.BuildRunner.Execute(IBuildOptions options) in C:\Users\Autobuild\workspace\AppStudio4.0-Build\modeler\Mendix.MxBuild\Build\BuildRunner.cs:line 87
    at Mendix.MxBuild.BuildRunnerMode.ExecuteBuild(IBuildOptions buildOptions) in C:\Users\Autobuild\workspace\AppStudio4.0-Build\modeler\Mendix.MxBuild\Build\BuildRunnerMode.cs:line 50
    at Mendix.MxBuild.BuildRunnerMode.Run(IList`1 args) in C:\Users\Autobuild\workspace\AppStudio4.0-Build\modeler\Mendix.MxBuild\Build\BuildRunnerMode.cs:line 39
    at Mendix.MxBuild.ApplicationRunner.Run(IEnumerable`1 args) in C:\Users\Autobuild\workspace\AppStudio4.0-Build\modeler\Mendix.MxBuild\ApplicationRunner.cs:line 42
    at Mendix.CommandLine.Shared.ProgramHelper.InitializeAndRun[T](Func`2 program, Assembly[] extraAssemblies) in C:\Users\Autobuild\workspace\AppStudio4.0-Build\modeler\Mendix.CommandLine.Shared\ProgramHelper.cs:line 24
```