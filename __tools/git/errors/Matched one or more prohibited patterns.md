# [Error] Matched one or more prohibited patterns

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
