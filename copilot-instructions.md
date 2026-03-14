# Copilot Prompt: Generate Code for RCOMMERZ Microservice (Best Practices Only)

You are generating code for a microservice in the RCOMMERZ platform. Follow these strict coding rules:

1. **Code Quality & Maintainability**
   - Write **readable, modular, and extensible** code
   - Follow **DRY** (Don’t Repeat Yourself) and **KISS** (Keep It Simple, Stupid) principles
   - Use **meaningful variable, function, and class names**
   - Keep code **simple, testable, and easy to refactor**
   - Apply **SOLID principles** where practical but prioritize readability

2. **Configuration & Environment**
   - Always fetch environment/configuration values via a **centralized config**
   - Avoid hardcoding secrets, URLs, or credentials
   - Provide sensible defaults and validation for config values

3. **Database & Migrations**
   - Include **database migrations** or schema management if applicable
   - Use clear naming conventions for tables, columns, and indexes
   - Keep queries maintainable and avoid repetition

4. **Code Extensibility**
   - Structure code for **future feature addition**
   - Modularize responsibilities into services, modules, or classes
   - Keep functions and classes **focused and single-responsibility**

5. **Output Only**
   - Generate **application code only**
   - Do not generate Dockerfiles, CI/CD configs, or infrastructure code
   - Make code **language-agnostic** where possible, or compatible with existing service runtime
