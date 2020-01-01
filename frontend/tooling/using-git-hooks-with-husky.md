# Husky

> Using git hooks with 🐶

[[GitHub repo](https://github.com/typicode/husky)]

**Example**

We have project which use linters or formatters like **Eslint** and/or **Prettier**. We're using specific commands for running linter and/or formatter in *package.json**

```json
{
  "scripts": {
    "lint": "eslint src --ext .js,.ts,.tsx",
    "prettier": "prettier --ignore-path .gitignore \"**/*.+(js|jsx|json|ts|tsx)\"",
    "format": "npm run prettier -- --write",
    "check-format": "npm run prettier -- --list-different",
    "validate": "npm run check-format && npm run lint"
  }
}
```

We can run command **npm run validate** every time before we commit. **husky** is used for such things.

If we catch errors after running command **validate** that means what we can't make commit until we fix that issues;

**Setup**

We need to install **husky** as dev dependency:

```bash
npm i --save-dev husky
```

After that we need to create **huskyrc** file:

```json
{
  "hooks": {
    "pre-commit": "npm run validate"
  }
}
```

