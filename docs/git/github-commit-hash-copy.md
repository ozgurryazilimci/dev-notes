# GitHub Commit Hash Copy Bookmarklet

This bookmarklet allows you to quickly copy the latest commit hash from a GitHub project page along with a
project-specific prefix.

## Code

Create a new bookmark in your browser and paste the following code as the URL:

```bash
javascript:(function() {
  var str = "!deploy " + [].slice.call(document.querySelectorAll(".text-right > code > a")).pop().innerHTML;
  var project = window.location.pathname.split("/")[2];
  switch (project) {
    case "custom-app":
      str += " custom/";
      break;
    case "another-custom-app":
      str += " ac/";
      break;
    default:
      str += " " + project + "/";
  }
  navigator.clipboard.writeText(str);
})();
```

## How It Works

1. Navigate to a GitHub project page.
2. Click the bookmark you created.
3. The script finds the most recent commit hash on the page.
4. Based on the project name, it appends a shorthand (e.g., `shop/`, `blog/`, `custom/`).
5. The final string (like `!deploy abc123 shop/`) is copied to your clipboard.

## Example

If you are on a repository with the path:  
`https://github.com/yourusername/shopping-app/commits/main`

And the latest commit hash is `abc1234`, clicking the bookmark will copy:

`!deploy abc1234 shop/`
