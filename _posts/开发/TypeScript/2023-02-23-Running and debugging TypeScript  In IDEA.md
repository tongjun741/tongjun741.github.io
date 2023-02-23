---
tags:
    - TypeScript
---

Running and debugging TypeScript  In IDEA

Running and debugging TypeScript﻿Ultimate

Last modified: 27 July 2020

With IntelliJ IDEA, you can run and debug client-side TypeScript code and TypeScript code running in Node.js.

For information on running and debugging TypeScript with Angular, see Running and debugging Angular applications.

Before running or debugging an application, you need to compile your TypeScript code into JavaScript. You can do that using the built-in TypeScript compiler and other tools, including ts-node for running TypeScript with Node.js, used separately or as part of build process.

During compilation, there can also be generated source maps that set correspondence between your TypeScript code and the JavaScript code that is actually executed. As a result, you can set breakpoints in your TypeScript code, launch the application with the run/debug configuration of the type JavaScript Debug (for client-side code) or Node.js, and then step through your original TypeScript code, thanks to generated sourcemaps.

Running a client-side TypeScript application﻿

You can write a client-side application in TypeScript, compile the code as described in Compiling TypeScript into JavaScript, and then run and debug your application exactly in the same way as client-side applications written in JavaScript. The only difference is that you can set breakpoints right in the TypeScript code.

1. Compile the TypeScript code into JavaScript.

1. In the editor, open the HTML file with a reference to the generated JavaScript file. This HTML file does not necessarily have to be the one that implements the starting page of the application.

1. Do one of the following:

- Choose View | Open in Browser from the main menu or press ⌥F2. Then select the desired browser from the list.

- Hover your mouse pointer over the code to show the browser icons bar: 

![](../../_resources/a2efab92c772488298c42293431dd94c.png)

 

![](../../_resources/c823a311de8145deaf616017149edea6.png)

 

![](../../_resources/358af744535a4d4d92e7e0ae2634d233.png)

 

![](../../_resources/7fa7578f8cd24566808da0bd0edfc658.png)

 

![](../../_resources/de5b412699914cfe9af73472475dadc2.png)

 Click the icon that indicates the desired browser.

Debugging a client-side TypeScript application﻿

Most often, you may want to debug a client-side application running on an external development web server, for example, powered by Node.js. If your application is running on the built-in IntelliJ IDEA server, see Running a client-side TypeScript application above, you can also debug it in the same way as JavaScript running on the built-in server.

Debug a TypeScript application running on an external web server﻿

1. Configure the built-in debugger as described in Configuring JavaScript debugger.

1. Configure and set breakpoints in the TypeScript code.

1. Run the application in the development mode. Often you need to run npm start for that.

Most often, at this stage TypeScript is compiled into JavaScript and source maps are generated, see Compiling TypeScript into JavaScript for details.

When the development server is ready, copy the URL address at which the application is running in the browser - you will need to specify this URL address in the run/debug configuration.

1. Choose Run | Edit Configuration from the main menu, click 

![](../../_resources/e1893a19e6e94314b75dbbf73667704b.png)

 on the toolbar and select JavaScript Debug from the list. In the Run/Debug Configuration: JavaScript Debug dialog that opens, specify the URL address at which the application is running. This URL can be copied from the address bar of your browser as described in Step 3 above.

1. Select the newly created configuration in the Select run/debug configuration list on the toolbar and click 

![](../../_resources/026458e39c084c9a919d985741815894.png)

. The URL address specified in the run configuration opens in the chosen browser and the Debug tool window appears.

1. In the Debug tool window, proceed as usual: step through the program , stop and resume the program execution, examine it when suspended, view actual HTML DOM, and so on .

Running and debugging a server-side TypeScript application with Node.js﻿

With IntelliJ IDEA, you can launch server-side TypeScript code on Node.js via the Node.js run configuration. Before running or debugging, your TypeScript code has to be compiled into JavaScript, as described in Compiling TypeScript into JavaScript.

Run server-side TypeScript with Node.js﻿

1. Compile your TypeScript code into JavaScript, see Compiling TypeScript into JavaScript for details.

1. Create a Node.js run/debug configuration:

1. From the main menu, choose Run | Edit Configuration, then in the Edit Configurations dialog, click 

![](../../_resources/e1893a19e6e94314b75dbbf73667704b.png)

 on the toolbar and select Node.js from the list.

1. In the Run/Debug Configuration: Node.js dialog that opens, specify the Node.js interpreter to use. In the JavaScript File field, specify the compiled file generated from the main file of the application that starts it.

Learn more from Creating a Node.js run/debug configuration.

1. Select the newly created Node.js configuration from the Select run/debug configuration list on the toolbar and click 

![](../../_resources/8f52b0bbfa7c4333970978d09bba187a.png)

 next to it.

Debug server-side TypeScript with Node.js﻿

1. Compile your TypeScript code into JavaScript, see Compiling TypeScript into JavaScript for details.

1. Set the breakpoints in the TypeScript code where necessary.

1. Create a Node.js run/debug configuration as described above.

1. Select the newly created Node.js configuration from the Select run/debug configuration list on the toolbar and click 

![](../../_resources/026458e39c084c9a919d985741815894.png)

 next to it. The Debug tool window opens.

1. Perform the steps that will trigger the execution of the code with the breakpoints.

1. Switch to IntelliJ IDEA, where the controls of the Debug tool window are now enabled.

Using ts-node﻿

If you need to run or debug single TypeScript files with Node.js, you can use ts-node instead of compiling your code as described in Compiling TypeScript into JavaScript.

Install ts-node﻿

- In the embedded Terminal (⌥F12) , type:

npm install --save-dev ts-node

Create a custom Node.js run/debug configuration for ts-node﻿

1. From the main menu, choose Run | Edit Configuration, then in the Edit Configurations dialog, click 

![](../../_resources/e1893a19e6e94314b75dbbf73667704b.png)

 on the toolbar and select Node.js from the list. The Run/Debug Configuration: Node.js dialog opens.

1. In the Node Parameters field, add --require ts-node/register.

1. Specify the Node.js interpreter to use. This can be a local Node.js interpreter or a Node.js on Windows Subsystem for Linux.

1. In the JavaScript File field, specify the file to run or debug. Depending on your workflow, you can do that explicitly or using a macro.

- If you are going to always launch the same TypeScript file, click 

![](../../_resources/af70a5bdbbf44f4b81b87939e0240ede.png)

 and select this file in the dialog that opens. By default, the run/debug configuration gets the name of the selected file.

- If you need to launch different files, type $FilePathRelativeToProjectRoot$. With this macro, IntelliJ IDEA will always launch the file in the active editor tab.

![c2c724533af164121aa2733ba206bea8.png](../../_resources/13bb41c062594a1a9d02c631217f1d71.png)
1. Optionally

To pass any additional parameters to ts-node (for example, --project tsconfig.json), add them in the Application parameters field.

1. Save the configuration.

Run a TypeScript file﻿

Depending on the way you specified your TypeScript file in the run/debug configuration, do one of the following:

- If you typed the filename explicitly, select the file in the Project tool window or open it in the editor and then select Run <run/debug configuration>.

Alternatively, select the required configuration from the list on the toolbar and click 

![](../../_resources/7eef9ccc9cef4775b4970268082a4a59.png)

 next to the list or press ⌃R.

- If you specified a macro, open a TypeScript file in the editor, select the newly created configuration from the list on the toolbar, and click 

![](../../_resources/7eef9ccc9cef4775b4970268082a4a59.png)

 or press ⌃R.

IntelliJ IDEA shows the output in the Run tool window.

Debug a TypeScript file﻿

1. In the TypeScript file to debug, set the breakpoints as necessary.

1. Depending on the way you specified your TypeScript file in the run/debug configuration, do one of the following:

- If you typed the filename explicitly, select the file in the Project tool window or open it in the editor and then select Debug <run/debug configuration>.

Alternatively, select the required configuration from the list on the toolbar and click 

![](../../_resources/026458e39c084c9a919d985741815894.png)

 next to the list or press ⌃D.

- If you specified a macro, open a TypeScript file in the editor, select the newly created configuration from the list on the toolbar, and click 

![](../../_resources/026458e39c084c9a919d985741815894.png)

 next to the list or press ⌃D.



https://www.jetbrains.com/help/idea/running-and-debugging-typescript.html#ws_ts_run_debug_server_side

