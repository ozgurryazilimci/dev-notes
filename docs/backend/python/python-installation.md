# Python Installation

This page will guide you through installing Python, setting up development environments, running Python scripts, and
exploring basic tools for data analysis.

---

## Installing Python

- **Download Python**  
  Go to [Python.org](https://www.python.org/downloads/) and download **Python 3.13.7** (latest stable version).

- **Install Python**
    - Run the installer.
    - **Important:** Check the box **"Add Python to PATH"** before clicking Install.

- **Verify Installation**  
  Open a terminal or command prompt and type:

```bash
python --version
```

You should see something like:

   ```
   Python 3.13.7
   ```

---

## Development Environments (IDEs)

There are several ways to write and run Python code. Here are some recommended options:

### PyCharm

1. IDE developed by JetBrains.
2. Great for projects, especially if you have an **.edu email** (can get free license).
3. To run a Python project in PyCharm:
    1. Open PyCharm and create a new project.
    2. Add a new Python file (`.py`) in the project.
    3. Write your code, e.g.: `print("Hello world!")`
    4. Right-click the file → **Run** → see output in the console.

### VS Code

- Lightweight editor, requires **Python extension**.
- Steps to run Python in VS Code:
    1. Install [VS Code](https://code.visualstudio.com/).
    2. Install **Python extension** from Extensions Marketplace.
    3. Create a `.py` file.
    4. Run the file in the integrated terminal.

### Anaconda

- Distribution including Python, many scientific libraries, and AI/ML tools.
- Ideal for data analysis and AI projects.
- Comes with **Jupyter Notebook** for interactive code.

### Google Colab

- Online IDE for Python.
- Great for data analysis without installing anything locally.
- Visit [Google Colab](https://colab.research.google.com/) and start coding.

---

## Running Python Scripts

1. Save your file with `.py` extension.
2. Open terminal/command prompt.
3. Navigate to the folder containing your file.
4. Run: `python your_file.py`
5. You should see the output in the terminal.

---

## Using Jupyter Notebook

Jupyter Notebook is excellent for data analysis.

- Launch via Anaconda Navigator or command line:

```bash
jupyter notebook
```

- Create a new Python notebook.
- You can write Python code in cells and run them individually.
- Example:

```python
import pandas as pd

# Load Excel data
df = pd.read_excel("data.xlsx")
print(df.head())
```

---

## Example: Simple Python Script

Sample code:

```python
# hello.py
import numpy

example = numpy.zeros((3, 2))
print(example)
```

Installation and Running:

```bash
pip install numpy
pip3 install numpy
python hello.py
```

---

## Fixing Jupyter Authentication and XSRF Issues (Short Version)

**Common Symptoms:**

- Browser asks for password or token repeatedly.
- Errors like `403: XSRF cookie does not match POST argument` or `_xsrf argument missing from POST`.

**Quick Fix Summary:**

- **Use the correct token URL**  
  When Jupyter starts, copy the full link shown in the terminal (it includes `?token=...`).

- **Clear cookies for `localhost`**  
  Remove cookies in your browser or open in Incognito mode.

- **Remove stored password**  
  Edit `~/.jupyter/jupyter_server_config.json` and delete lines like:

```json
"IdentityProvider": {
"hashed_password": "argon2:..."
}
```

Save and restart Jupyter.

- **Disable password/token locally (optional)**  
  Add this to `~/.jupyter/jupyter_notebook_config.py`:

```python
c.NotebookApp.token = ''
c.NotebookApp.password = ''
c.ServerApp.password = ''
c.ServerApp.token = ''
c.PasswordIdentityProvider.hashed_password = ''
c.ServerApp.disable_check_xsrf = True  # local only!
```

- **Restart Jupyter**

```bash
jupyter notebook stop 8888
jupyter notebook
```

- **If still broken**  
  Reset config:

```bash
mv ~/.jupyter ~/.jupyter.backup
jupyter notebook --generate-config
```

✅ *Now Jupyter should open directly at `http://localhost:8888` without asking for a token or password.*


---

## References

- [Official Python Docs](https://docs.python.org/3/)
- [Python Tutor](http://pythontutor.com)
- [TutorialsPoint Python 3](https://www.tutorialspoint.com/python3/index.htm)
- [Python Yazbel](https://python.yazbel.com)
- [W3Schools Python](https://www.w3schools.com/python/)
- [NumPy](https://numpy.org/)
- [TechIstanbul Github](https://github.com/sinanurun/TechIstanbul_Python_Bootcamp)