---
marp: true
theme: gaia
_class: lead
paginate: true
backgroundColor: #fff
backgroundImage: url('Assets/background.svg')
---

![bg left:40% 80%](Assets/codeastu_ciklum.png)

# **Javascript Essentials**

---
# JavaScript Study Materials

A comprehensive collection of JavaScript study materials covering essential concepts and advanced topics. This repository contains detailed explanations, code examples, and practical exercises for mastering JavaScript fundamentals.

---

## 📚 Topics Covered

### 1. **Closures** (`closures.md`)
- Understanding closure concepts and how they work
- Data privacy and encapsulation using closures
- Function factories and practical closure patterns
- Common closure use cases and examples
- Advanced closure techniques
---
### 2. **Functions** (`functions.md`)
- Higher-order functions and function composition
- Callback functions and execution patterns
- Function declarations vs expressions
- Arrow functions and their behavior
- First-class functions in JavaScript
---
### 3. **Objects** (`Objects.md`)
- Different ways to create objects in JavaScript
- Object constructors and factory functions
- Object literals and property access
- Class-based object creation
- Object methods and prototypes
---
### 4. **`this` Keyword** (`this.md`)
- Understanding the `this` context in JavaScript
- `this` binding in different scenarios
- Method invocation and `this` behavior
- Arrow functions and lexical `this`
- Common `this` pitfalls and solutions
---
### 5. **Hoisting** (`hoisting.md`)
- Variable and function hoisting in JavaScript
- Temporal Dead Zone (TDZ) with let and const
- Function declarations vs function expressions
- Hoisting behavior with different variable declarations
- Common hoisting pitfalls and best practices
---
### 6. **Scopes and Variables** (`scopes.md & variables.md`)
- Global, function, and block scope in JavaScript
- `var`, `let`, and `const` differences
- Scope chain and lexical scoping
- Variable shadowing and scope pollution
- Best practices for variable declaration
---
## 📁 Repository Structure

```
javascript-study/
├── README.md              # This file
├── closures.md            # Closures study material
├── functions.md           # Functions study material
├── Objects.md             # Objects study material
├── this.md                # this keyword study material
├── hoisting.md            # Hoisting study material
├── scopes.md    # Scopes study material
├── variables.md    # Variables study material
└── Assets/                # Supporting assets
    ├── background.svg     # Background image
    └── codeastu_ciklum.png # Logo
```
---
## 🚀 Getting Started

1. **Clone the repository:**
   ```bash
   git clone https://github.com/pikulad/js-study-materials.git
   cd js-study-materials
   ```

2. **Study the materials:**
   - Open any `.md` file to read the study material
   - Each file contains detailed explanations with code examples
   - Practice the examples in your browser console or Node.js

3. **Access PDF versions:**
   - PDFs are automatically generated via GitHub Actions
   - Download from the [Actions tab](https://github.com/pikulad/js-study-materials/actions) artifacts
   - Or access via GitHub Pages at `https://pikulad.github.io/js-study-materials/pdfs/`

4. **Local development:**
   ```bash
   # Install dependencies
   npm install
   
   # Generate PDFs locally
   npm run build:pdf
   
   # Preview slides locally
   npm run preview
   ```
---
## 📖 How to Use This Repository

### For Beginners
- Start with `variables.md` `hoisting.md` `scopes.md` to understand basic concepts
- Move to `objects.md` to learn about object creation and manipulation
- Study `this.md` to understand context and binding
- Finally, tackle `closures.md` for advanced concepts
---
### For Intermediate Developers
- Use these materials as a reference for JavaScript fundamentals
- Practice the code examples to reinforce your understanding
- Focus on areas where you need more clarity

### For Advanced Developers
- Review the materials to ensure comprehensive understanding
- Use as teaching material for mentoring others
- Reference for technical interviews and code reviews
---
## 💡 Key Learning Objectives

After studying these materials, you should be able to:

- ✅ Understand and implement closures in JavaScript
- ✅ Work with higher-order functions and callbacks
- ✅ Create and manipulate objects using various methods
- ✅ Master the `this` keyword and its different contexts
- ✅ Understand hoisting behavior and avoid common pitfalls
- ✅ Master variable scoping and choose appropriate declarations
- ✅ Write more maintainable and efficient JavaScript code
- ✅ Debug common JavaScript issues related to these concepts
---
## 🤖 Automated PDF Generation

This repository uses GitHub Actions to automatically convert Marp Markdown files to PDF format:

- **Trigger**: Automatically runs on every push to main/master branch
- **Output**: Generates PDF versions of all study materials
- **Access**: Download from Actions artifacts or GitHub Pages
- **Manual**: Can be triggered manually from the Actions tab

### Workflow Features:
- ✅ Converts all `.md` files (except README) to PDF
- ✅ Preserves Marp themes and styling
- ✅ Creates downloadable artifacts
- ✅ Publishes to GitHub Pages
- ✅ Runs on every content update

## 🔗 Additional Resources

Each study material includes references to:
- [Eloquent JavaScript](https://eloquentjavascript.net/) - Comprehensive JavaScript guide
- [MDN Web Docs](https://developer.mozilla.org/en-US/docs/Web/JavaScript) - Official JavaScript documentation
- [Marp](https://marp.app/) - Markdown presentation ecosystem
- Other relevant learning resources
---
## 🤝 Contributing

This is a personal study repository, but suggestions and improvements are welcome! If you find any errors or have additional examples to share, feel free to:

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Submit a pull request
---
## 📝 License

This project is for educational purposes. Feel free to use these materials for learning and teaching JavaScript concepts.

## 📞 Contact

Created by [@pikulad](https://github.com/pikulad) as part of JavaScript learning journey.

---

**Happy Learning! 🎉**

*Master these JavaScript fundamentals and you'll be well on your way to becoming a proficient JavaScript developer.*
