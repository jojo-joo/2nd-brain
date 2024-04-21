
- Download the `C/C++ extension` and the `Clang-format` extension from the VSCode marketplace.
- Navigate to the directory where the `Clang-format` executable is located. This directory can typically be found at `C:\Users\Administrator\.vscode\extensions\ms-vscode.cpptools-1.19.4-win32-x64\LLVM\bin`.
- Run the following command to generate a `.clang-format` file with the LLVM style configuration: 
    ``` shell
    .\clang-format.exe -style="llvm" -dump-config > .clang-format
    ```
- This command will create a `.clang-format` file, which you can then modify to customize the formatting style.

- In VSCode, input `ctrl + ,` , open settings
- Enter `Extensions -> C/C++ -> Formatting`
- C_Cpp: Clang_format_path：`C:\Users\Administrator\.vscode\extensions\ms-vscode.cpptools-1.19.4-win32-x64\LLVM\bin\clang-format.exe`
- C_Cpp: Clang_format_style：`C:\Users\Administrator\.vscode\extensions\ms-vscode.cpptools-1.19.4-win32-x64\LLVM\bin\.clang-format`
