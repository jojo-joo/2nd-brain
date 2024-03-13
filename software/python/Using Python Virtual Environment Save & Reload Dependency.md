1. **Navigate to the project directory**: First, make sure you are in your project directory. You can use the `cd` command to navigate to your project directory.

   ```bash
   cd /path/to/your/project
   ```

2. **Create the virtual environment**: In your project directory, run the following command to create a virtual environment named `venv` (you can replace `venv` with any name you prefer).

   ```bash
   python3 -m venv venv
   ```
   or if meet something error in ubuntu20.04, please try
   ```
   python3.11 -m venv --without-pip venv
   ```

   This will create a virtual environment named `venv` in the current directory.

3. **Activate the virtual environment**: Once the virtual environment is created, you need to activate it. Activating the virtual environment adds it to your Shell session so that you can use the Python and packages installed in the virtual environment for your project.

   On Unix/Linux/macOS systems:

   ```bash
   source venv/bin/activate
   ```

   On Windows systems:

   ```bash
   venv\Scripts\activate
   ```

   Once activated, you will see `(venv)` or the name of the virtual environment at the beginning of the command prompt.

4. **Use Python and pip within the virtual environment**: Now you can use Python and the `pip` command within the virtual environment. Any packages you install within this virtual environment will be associated with it and won't affect the global Python environment of your system.

5. **Deactivate the virtual environment**: When you are done working, you can deactivate the virtual environment by simply typing `deactivate`.

   ```
   deactivate
   ```

6. **Remove all installed global packages**: Please note that this command will remove all packages in the current Python environment, including system-level packages, so use it with caution.

   ```bash
   pip freeze > packages.txt
   pip uninstall -r packages.txt -y
   ```

7. **To save the dependencies of your Python project**: you can use the `pip freeze` command along with redirection to save them to a `requirements.txt` file.

   ```
   pip freeze > requirements.txt
   ```
   
   This command will save all currently installed packages and their versions to a file named `requirements.txt` in the current directory.
   
8. **To recreate the environment**: you can use the following command with the `requirements.txt` file:

   ```
   pip install -r requirements.txt
   ```
   
   This command will install all the dependencies listed in the `requirements.txt` file, effectively recreating the environment.
   
   Remember to run these commands in the environment where you have your project dependencies installed.
