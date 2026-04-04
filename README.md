# CometAPI: Integration Guide

This guide is for developers using the `Comet Executor` to add execution capabilities to their Windows Forms or WPF projects. You don't need to worry about the internal logic—the API handles the heavy lifting, including auto-updates and memory management.

## 1. Installation
1.  In Visual Studio, right-click **References** in your Project Explorer.
2.  Select **Add Reference...** and browse for the `CometAPI.dll` or `CometAPI.NetCore.dll` for NetCore.
3.  **CRITICAL:** Go to your **Project Properties > Build** and set the **Platform Target** to `x64`. 
    * *Note: Our loader core is 64-bit. If your project is set to "Any CPU" or "x86", the API will crash when you try to Attach.*

---

## 2. Setting Up Your Config
You only need to do this once. Usually, you’d put this in your `Form_Load` or the `Form1` constructor. This tells the API how you want your executor to identify itself.

```csharp
using CometAPI; // add at the top with others

public Form1()
{
    InitializeComponent();

    Functions.CurrentConfig.SetConfig(
        agent: "YourName/1.0",
        name: "ExecutorName",
        identity: 3,
        autoUpdate: true,
        method: ExecutionMethod.DLLExports
    );
}
```

---

## 3. Handling the "Attach"
The `Attach` method is where the magic happens. It checks the web for updates, ensures the `/bin` folder exists, and prepares the loader for execution.

**Use `async` and `await`** so your application doesn't freeze while it's checking for updates.

```csharp
private async void Attach_Click(object sender, EventArgs e)
{
    await Functions.Attach();
}
```

---

## 4. Running Scripts
Once attached, running a script is just one line of code.

```csharp
private void Execute_Click(object sender, EventArgs e)
{
    Functions.Execute(textbox.Text);
}
```

---

## Troubleshooting
* **"Attempted to read or write protected memory"**: This means your project is set to `x86`. Go to Project Properties and change it to `x64`.
* **"DllNotFoundException"**: The API expects a folder named `bin` in your app's directory. If you are debugging in Visual Studio, make sure that folder is inside `bin/Debug` or `bin/Release`.
* **Updates not working**: Ensure you have an active internet connection so the API can reach GitHub to check the version string.
---

### Pro Tip
The `ExecutorVersion` is read-only. If the developer of the API pushes a new version to the server, your app will automatically ask the user to download the update the next time they click **Attach**. You don't have to do anything!
