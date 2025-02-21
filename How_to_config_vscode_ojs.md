# Setting up enviroment

**Configure VSCodium/VSCode**

You need clone ojs project an then, include the path in your environment:

```bash
git clone https://github.com/pkp/ojs --recurse-submodules -b stable-3_4_0

```

Install intelephense plugin for code.

**In VS Code Settings**:

```
1.  Open settings (`Ctrl + ,` or `Cmd + ,`).
    
2.  Search for `intelephense.environment.includePaths`.
    
3.  Add the path to the `PKP` directory (e.g., `"/home/user/dev/ojs/"`).

```

```json
    "intelephense.environment.includePaths": [
      "${workspaceFolder}/"
    ]

```

----------

-   “this step i dont know if is correct”

copy composer.json from ojs/lib/pkp/ to your working directory  
and run composer install or if it fails:

```bash
composer  install --ignore-platform-reqs

```

Maybe you need to restart _VScode_

😀
