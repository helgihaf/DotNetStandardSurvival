# Code Analysis and Code Styling at Build Time

There are a few steps to this:
1. Create an .editorconfig file specifying the rules you want or don't want. This will be picked up by Visual Studio, VS Code and Ryder. This will NOT be enough to enforce the rules at build time.
2. Add this property to the relevant projects: `<EnforceCodeStyleInBuild>true</EnforceCodeStyleInBuild>`
3. Add this package to the relevent projects: Microsoft.CodeAnalysis.CSharp.CodeStyle

This will be enough to fail builds that don't adhere to your rules. Some rules however (suggestions) will not fail the build and will not generate warnings at build time either.

If you want a report with warnings, you have to resort to other means:

* JetBrains inspectcode: jb inspectcode.
* https://github.com/dotnet/roslynator
