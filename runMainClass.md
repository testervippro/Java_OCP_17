# 🧩 Goal

Normally, you have to type something long like:

```bash
mvn exec:java -Dexec.mainClass=com.thoaikx.dataprovider.Main -Dexec.classpathScope=test -Dexec.args="--debug"
```

That’s a lot to remember. 😩

We’ll create a **shortcut** so you can simply type:

```bash
mvn mainClass=com.thoaikx.dataprovider.Main scope=test --debug
```

and it will automatically expand to the full command above. 🚀

---

# ⚙️ Step 1. Open Your Shell Config File

Depending on your shell:

### macOS (Zsh default)

```bash
nano ~/.zshrc
```

### Linux or Bash

```bash
nano ~/.bashrc
```

---

# 🧱 Step 2. Add This Function

Paste this function **at the bottom** of your config file:

```bash
mvn() {
  if [[ "$1" == "mainClass="* ]]; then
    local mainClass="${1#mainClass=}"
    shift
    local scopeArg=""
    local args=""

    # Check if "scope=test" is passed
    for arg in "$@"; do
      if [[ "$arg" == "scope="* ]]; then
        local scope="${arg#scope=}"
        if [[ "$scope" == "test" ]]; then
          scopeArg="-Dexec.classpathScope=test"
        fi
      else
        args+="$arg "
      fi
    done

    # Run Maven with detected options
    if [[ -n "$scopeArg" ]]; then
      echo "🧪 Running with classpath scope: test"
      command mvn exec:java -Dexec.mainClass="$mainClass" $scopeArg -Dexec.args="$args"
    else
      echo "🚀 Running with default (main) scope"
      command mvn exec:java -Dexec.mainClass="$mainClass" -Dexec.args="$args"
    fi
  else
    command mvn "$@"
  fi
}
```

---

# 🔁 Step 3. Reload Your Shell

After saving, apply the changes:

```bash
source ~/.zshrc
```

or

```bash
source ~/.bashrc
```

✅ This makes the new `mvn()` command available in your terminal.

---

# 🚀 Step 4. Use the Shortcut

Now you can run Java classes directly with simplified commands.

---

## 🧠 Example 1 — Run Main Class (Default Scope)

```bash
mvn mainClass=com.example.App
```

👉 Expands to:

```bash
mvn exec:java -Dexec.mainClass=com.example.App
```

Output:

```
🚀 Running with default (main) scope
```

---

## 🧪 Example 2 — Run a Test Class

```bash
mvn mainClass=com.thoaikx.dataprovider.Main scope=test
```

👉 Expands to:

```bash
mvn exec:java -Dexec.mainClass=com.thoaikx.dataprovider.Main -Dexec.classpathScope=test
```

Output:

```
🧪 Running with classpath scope: test
```

---

## ⚙️ Example 3 — Run With Program Arguments

```bash
mvn mainClass=com.thoaikx.dataprovider.Main scope=test --debug mode=prod
```

👉 Expands to:

```bash
mvn exec:java -Dexec.mainClass=com.thoaikx.dataprovider.Main -Dexec.classpathScope=test -Dexec.args="--debug mode=prod"
```

---

# 💡 Step 5. Verify

To confirm your function is active:

```bash
type mvn
```

✅ Should show:

```
mvn is a function
```

If it says `/opt/homebrew/bin/mvn`, you haven’t reloaded the shell yet — run `source ~/.zshrc`.

---

# 🧾 Step 6. Summary

| Task           | Command                                            | Expands To                                                                                        |
| -------------- | -------------------------------------------------- | ------------------------------------------------------------------------------------------------- |
| Run main class | `mvn mainClass=com.example.App`                    | `mvn exec:java -Dexec.mainClass=com.example.App`                                                  |
| Run test class | `mvn mainClass=com.example.App scope=test`         | `mvn exec:java -Dexec.mainClass=com.example.App -Dexec.classpathScope=test`                       |
| Run with args  | `mvn mainClass=com.example.App scope=test --debug` | `mvn exec:java -Dexec.mainClass=com.example.App -Dexec.classpathScope=test -Dexec.args="--debug"` |

---

# 🧰 Step 7. (Optional) Rename It to Avoid Conflicts

If you want to keep the original Maven command untouched,
rename the function to something like `runMainClass` instead:

```bash
runMainClass() {
  # same content as above
}
```

Then use it like:

```bash
runMainClass mainClass=com.thoaikx.dataprovider.Main scope=test
```

---


Would you like me to extend this further so it also supports **file paths** (like `src/test/java/com/example/App.java`) and automatically converts them to the correct `com.example.App` class name?
