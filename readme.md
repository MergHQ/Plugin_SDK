Plugin SDK for CryEngine SDK
=====================================
This is still a work in progress!

Purpose is to create a tailored way for automatically loading plugins.
For redistribution please see license.txt.

Stable Release:
* CryEngine 3 FreeSDK Version 3.4
* DirectX 11 and 9
* 32/64 Bit

Feature requests/latest version on github.
https://github.com/hendrikp/Plugin_SDK

Extraction
==========
Extract the Files to your cryengine SDK Folder
so that the Code and BinXX directory match up.
(Select the correct FreeSDK version)

If you have a custom GameDll then you will need
to recompile it see C++ Integration.

CVars
=====
* pm_list
  List one info row for all plugins
* pm_dump
  Dump info on a specific plugins
* pm_dumpall
  Dump info on all plugins
* pm_unload
  Unload a specific plugin using its name
* pm_unloadall
  Unload all plugins with the exception of the plugin manager
* pm_reload
  Reload a specific plugin using its path
* pm_reloadall
  Reload all plugins

C++ Integration
===============
Follow these steps to integrate the Plugin SDK into your GameDLL.

Compiler Settings
-----------------
* Open the Solution CryEngine_GameCodeOnly.sln
* Open CryGame Properties
* Set the Dropdowns to All Configurations, All Platforms
* Add to C/C++ -> General -> Additional Include Directories:

    ;$(SolutionDir)..\Plugin_SDK\inc

* Apply those properties and close the dialog

Source Files
------------
* Inside the CryGame Project (Solution Explorer)
  Open the following File: Startup Files / GameStartup.cpp
* Add behind the existing includes the following:

    #include <IPluginManager_impl.h>

* Find the function CGameStartup::Init
* Add in the middle of the function (before "REGISTER_COMMAND("g_loadMod", RequestLoadMod,VF_NULL,"");") the following:

	PluginManager::InitPluginManager(startupParams, "3.4"); // Update the CryEngine SDK Version!
	PluginManager::InitPluginsBeforeFramework();
    REGISTER_COMMAND("g_loadMod", RequestLoadMod,VF_NULL,""); // <--

* The initialization of the plugin manager can only happen once because its a singleton.
  Also don't forget to supply the SDK Version as parameter so the plugins can check compatiblity.
* Add at the end of the function (before "return pOut;") the following:

    PluginManager::InitPluginsLast();
    return pOut; // <--

Contributing
------------
* Fork it
* Create a branch (`git checkout -b my_PluginSDK`)
* Commit your changes (`git commit -am "Added ...."`)
* Push to the branch (`git push origin my_PluginSDK`)
* Open a Pull Request