# Vulkan Tutorial

This project is a space for me to keep track of what I learn with [Vulkan](https://vulkan-tutorial.com/).

## Setup

My setup was helped by following instructions provided in the [development environment](https://vulkan-tutorial.com/en/Development_environment) section, however, I had to customize tasks.json for my VSCode project in order for static linking to work.

Both GCC and G++ worked with the following addition in their args section:

    /* includes */
    "-IC:\\VulkanSDK\\1.3.261.1\\Include",
    "-I${workspaceFolder}\\src\\libs\\glfw-3.3.8.bin.WIN64\\include",
    "-I${workspaceFolder}\\src\\libs\\glm-0.9.9.8",
    /* library directories & names ('Ldir' & '-larchive'). */ 
    "-LC:\\VulkanSDK\\1.3.261.1\\Lib",
    "-L${workspaceFolder}\\src\\libs\\glfw-3.3.8.bin.WIN64\\lib-mingw-w64",
    "-lvulkan-1",
    "-l:libglfw3.a",
    /* These come from MinGW which is on the path. */
    "-l:libgdi32.a",
    "-l:libstdc++.a"

The additional libraries to include where determined by using grep within the MinGW directory based on the undefined references in the build output.

The GLFW readme provided instructions on statically linking the appropriate '.a' file. 

Additional help came from:

> [how-can-i-run-an-exe-with-dlls-in-separate-directory](https://stackoverflow.com/questions/5765986/how-can-i-run-an-exe-with-dlls-in-separate-directory)
>> first of all there is no need to copy your exe file to your project dir, whereever your .exe file is created when you are debuging your project the running dir would be your project dir. and after that when you are trying to import the dll if you look for it relatively windows first search for that dll in your running dir then then it checks if it can find the dll in the whatever directory defined system PATH variable, but if you check for a absolute address there will be no looking. so the first trick is to set all your dll pathes abslote so that there will be no searching and dll are imported easily but there will be a lot of problems if you want to move your application to another computer (eg HINSTANCE hDLL = LoadLibrary(L"C:\\mydll.DLL");) . second you can give your dll pathes relative to the running dir (not the application path, these 2 may differ) and you can also specify directory for that (eg. HINSTANCE hDLL  LoadLibrary("..\\dlls\\mydll.dll")

> [where-to-place-and-how-to-include-dll-files-in-c-project](https://stackoverflow.com/questions/16064677/where-to-place-and-how-to-include-dll-files-in-c-project)
>> "There's nothing wrong with this approach, and it's probably the quickest way to get your application up and running. Your application needs these libraries, so putting them in the same folder allows it to find them. There are of course always alternatives! Static linking: To avoid having a bunch of dlls that have to exist with your application, you could use static linking. You link the .lib files at link-time rather than linking to the .dll files at run-time. Makes your .exe larger, but means you may be able to distribute a single file. Placing Dlls in a different folder: The dlls do not have to be in the same folder as the .exe, though that normally makes the most sense. Windows will search several folders when looking for a .dll at run-time, so you could put the dlls in the current directory, a system directory (definitely not recommended), or another directory in your PATH environment variable. Actually none of those locations are recommended! Putting them in the same folder as the .exe is the safest, because generally you have control of that folder. The specific rules for how Windows searches for a .dll are outlined here: http://msdn.microsoft.com/en-ca/library/windows/desktop/ms682586(v=vs.85).aspx. Custom Build Step: I don't like manually putting files in my debug or release folders. I like to think of the debug folder as something I can blow away and rebuild at any time, and I like to have a one-step build process that puts everything where it needs to be so I can easily build on any machine. So I will usually create a custom build step that copies the required .dlls from a "source" folder (in source control) into my output folder."
