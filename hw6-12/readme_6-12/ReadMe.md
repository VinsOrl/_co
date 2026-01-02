# Nand2Tetris: From Logic Gates to Operating System
**A Complete Implementation of the Hack Computer Architecture**

## 📖 Project Overview
This repository contains my complete journey through the **Nand2Tetris** curriculum. I have built the software stack for the Hack computer, starting from a machine-level Assembler and culminating in a high-level Operating System.

---

## 🛠️ Software Stack Implementations

### Project 06: The Assembler
I developed a Python-based assembler to translate Hack assembly language into binary machine code. 
* **Two-Pass Logic:** The first pass builds a `SymbolTable` for labels; the second pass generates the 16-bit binary instructions.
* **Features:** Handles predefined symbols (R0-R15, SCREEN, KBD), user-defined variables, and complex C-instructions.



### Projects 07 & 08: VM Translator
A two-part project building a translator for the Hack Virtual Machine.
* **Stack Logic:** Implemented a stack-based architecture for arithmetic (`add`, `sub`, etc.) and memory access.
* **Functionality:** Supports program flow (branching), function calling conventions (saving/restoring frames), and recursion.
* **Bootstrap:** Automatically initializes the system and calls `Sys.init`.



### Projects 10 & 11: Jack Compiler
A full-scale compiler that translates high-level Jack code into VM instructions.
1.  **Tokenizer:** Lexical analysis of source text.
2.  **Compilation Engine:** Recursive-descent parsing.
3.  **Symbol Table:** Managing scope for static, field, and local variables.
4.  **VM Writer:** Direct translation to stack-based VM commands.

---

## 💻 Technical Implementation Example (Assembler)

```python
# Part of the Project 06 Assembler Logic
def second_pass(self, lines):
    machine_code = []
    for line in lines:
        if line.startswith("@"):
            # A-Instruction Logic
            val = line[1:]
            address = self.symbol_table.get(val, val)
            machine_code.append(format(int(address), '016b'))
        else:
            # C-Instruction Logic (dest=comp;jump)
            # ... translation via mnemonic tables ...
            pass
    return machine_code
```

## 🖥️ Project 12: The Operating System (OS)
The final layer of the stack provides essential library services to Jack programs.

| OS Class | Functionality | Interdependency |
| :--- | :--- | :--- |
| **Memory** | Heap management and RAM access | Foundation for all classes |
| **Math** | Arithmetic operations (Mult, Div, Sqrt) | Uses Array/Memory |
| **Screen** | Graphics primitives (Lines, Circles) | Uses Math/Memory |
| **Output** | Character and text rendering | Uses String/Array/Memory |
| **Keyboard** | User input handling | Uses String/Memory |
| **Sys** | Execution control and initialization | Orchestrates all classes |

---

## 🚀 How to Use

### 1. Compile Jack Files:
```powershell
./JackCompiler.bat path/to/jack_project
```

### Translate VM to ASM:

**PowerShell**
```powershell
python 8.py path/to/project_folder
```

### Assemble to Binary:

**PowerShell**
```powershell
python 6.py Program.asm
```

## 🎓 Acknowledgments

These projects were completed with the support of **AI (Google Gemini)** for logic optimization, debugging, and architectural guidance. This journey provided deep insight into the "abstraction cliff"—understanding exactly how high-level code is progressively stripped down into electrical signals, and how hardware and software integrate to create a functional computing environment.