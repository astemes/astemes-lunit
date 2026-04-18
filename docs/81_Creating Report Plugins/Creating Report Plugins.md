# Creating Report Plugins
LUnit was designed to be extendable through plugins and a lot of the features of LUnit are implemented as plugins behind the scenes.
If you'd like to create your own reporting plugin, this can be done following the guide below.
The only requirements for a reporting plugin is that it implements two specific interfaces and is placed in a specific folder within vi.lib.
With the release of LUnit 2.0, the reporting plugin design was updated to make it simpler and more developer friendly.
Plugins made for earlier versions of the framework need to be updated to adhere to the new design, please see section below.

## Getting started
To create a custom reporting plugin, you will need to create a class which implements the `LUnit Reporting Plugin.lvclass` interface.
The easiest way to get started is to copy one of the existing plugins, which are located at `C:\Program Files (x86)\National Instruments\LabVIEW 202X\vi.lib\Astemes\LUnit\plugins`.
It is recommended to use LabVIEW 2020 to ensure backward compatibility.

## Implementing the Reporting Plugin Interface
The report interface is very simple and is called into by the report generator.
The `LUnit Start Test.vi` is called when a new test execution is started, `LUnit Result.vi` is called on the fly for each result generated, and `LUnit Stop Test.vi` is called when the execution is done.

Data can be passed between the class method calls by bundling data into the class data cluster.
One important limitation is that new references cannot be opened during the `LUnit Result.vi` or they might be prematurely closed by the LabVIEW run-time.

It is important to understand the different types of results which are sent to the `LUnit Result.vi`.
Please refer to the Framework Architecture document and the unit tests in the LUnit project for the native report formats.

## Plugin Configuration
When the user activates the plugin, you have the option to provide a configuration dialog.
Please refer to the built in plugins as examples for how to implement this.

## Deploying your plugin
To deploy the plugin, it should be placed in the plugin directory at `C:\Program Files (x86)\National Instruments\LabVIEW 202X\vi.lib\Astemes\LUnit\plugins`.
The plugin name should begin with the `LUnit` prefix. 
The `.lvclass` file should be directly in the `plugins` directory, but the member VI:s of the class will need to be placed in a sub directory to avoid naming conflicts.

After doing this, the plugin should appear in the drop-down menu in the reporting configuration dialog.
To automate this, making a VIPM package is recommended and sharing it with the community through VIPM is encouraged.

## Upgrading report plugins from LUnit version 1.x to version 2.x
If you have developed a plugin in version 1.x of LUnit it will need to be updated to adhere to the new design.
If you open the report written in the old version, you will receive errors in LabVIEW and below is a step by step guide to resolve this.

1. Update the inheritance tree of the plugin. 
When you open the class properties of the plugin, you will see missing classes and interfaces.
Change the inheritance to use the LabVIEW Object as parent class and remove the parent interfaces `LUnit Report Generation.lvclass` and `LUnit Report Interface.lvclass`.
Make your plugin inherit the new `LUnit Reporting Plugin.lvclass` interface.
2. Update the methods of the plugin to follow the new convention.
- Rename `LUnit Open Report File.vi` to `LUnit Start Test.vi`
- Rename `LUnit Handle Result.vi` to `LUnit Result.vi`
- Rename `LUnit Close Report File.vi` to `LUnit Stop Test.vi`
These three methods should have the regular Dynamic Dispatch template connector pane with a Dynamic Dispatch Input and Output, as well as the error terminals, and they need to be made reentrant. The `LUnit Start Test.vi` also needs a string array input, where a list of tests is provided when the test is started.
3. The path to save the report is no longer handled by the framework and the plugin must determine this path.
It is recommended to override the `Configure.vi` to let the user specify a path to save reports.
