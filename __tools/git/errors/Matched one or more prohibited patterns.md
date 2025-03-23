# [Error] Matched one or more prohibited patterns

## Problem
```bash
~/github$ ./batch_git_push
TARGET_REPOSITORIES: my_web
cd /home/thekim/github/my_web
git add . && git commit -m 'Add new stuff by batch_git_push' && git push
public_html/index.html:782:    <script async defer src="https://maps.googleapis.com/maps/api/js?key=AIzaSyCtme10pzgKSPeJVJrG1O3tjR6lk98o4w8&callback=initMap"></script>

[ERROR] Matched one or more prohibited patterns

Possible mitigations:
- Mark false positives as allowed using: git config --add secrets.allowed ...
- Mark false positives as allowed by adding regular expressions to .gitallowed at repository's root directory
- List your configured patterns: git config --get-all secrets.patterns
- List your configured allowed patterns: git config --get-all secrets.allowed
- List your configured allowed patterns in .gitallowed at repository's root directory
- Use --no-verify if this is a one-time false positive
~/github$
```

## Hint
By Gemini

The error message you're seeing indicates that your Git pre-commit hooks are configured to prevent you from committing sensitive data, such as API keys. In your case, the Google Maps API key `AIzaSyCtme10pzgKSPeJVJrG1O3tjR6lk98o4w8` is being flagged as a prohibited pattern.

Here's a breakdown of the problem and solutions:

**Problem:**

* Your Git repository has a pre-commit hook (likely from a tool like `git-secrets`) that scans for patterns that resemble sensitive information.
* The Google Maps API key in `public_html/index.html` matches a pattern defined in the hook, causing the commit to fail.

**Solutions:**

1.  **Mark the API key as allowed (if it's a false positive):**

    * If you're absolutely certain that this API key is not sensitive or that it's safe to commit (e.g., if it's a test key), you can tell Git to ignore it.
    * **Using `git config --add secrets.allowed`:**
        ```bash
        git config --add secrets.allowed 'AIzaSyCtme10pzgKSPeJVJrG1O3tjR6lk98o4w8'
        ```
        This command adds the specific API key to the list of allowed patterns in your Git configuration.
    * **Using `.gitallowed` file:**
        * Create a file named `.gitallowed` in the root directory of your repository.
        * Add the API key (or a regular expression that matches it) to the file, one pattern per line.
        ```
        AIzaSyCtme10pzgKSPeJVJrG1O3tjR6lk98o4w8
        ```
        This method is more flexible for managing multiple allowed patterns.

2.  **Remove the API key from your code:**

    * The best practice is to avoid committing API keys directly to your repository.
    * Store the API key in an environment variable or a configuration file that is not committed to Git.
    * Modify your code to retrieve the API key from the environment or configuration.
    * For example, in Javascript, you could store the API key in an environment variable and access it like this.
    ```javascript
    const apiKey = process.env.GOOGLE_MAPS_API_KEY;
    const script = document.createElement('script');
    script.src = `https://maps.googleapis.com/maps/api/js?key=${apiKey}&callback=initMap`;
    script.async = true;
    script.defer = true;
    document.head.appendChild(script);
    ```
    * Then on the server where the code is run, set the environment variable.

3.  **Use `--no-verify` (one-time solution):**

    * If you need to bypass the pre-commit hook for a single commit (e.g., for a quick fix), you can use the `--no-verify` flag.
    ```bash
    git push --no-verify
    ```
    * **Caution:** This is generally discouraged, as it bypasses security checks. Only use it if you fully understand the risks and have a temporary need.

4.  **List configured patterns:**

    * To see what patterns are being checked by git secrets, use:
    ```bash
    git config --get-all secrets.patterns
    ```
    * To see the allowed patterns, use:
    ```bash
    git config --get-all secrets.allowed
    ```
    * To see the allowed patterns in .gitallowed, simply open the file.

**Recommendation:**

* Prioritize removing the API key from your code and using environment variables or configuration files.
* If removing the key is not immediately feasible, use the `.gitallowed` file to manage allowed patterns.
* Avoid using `--no-verify` unless absolutely necessary.

### Solution
