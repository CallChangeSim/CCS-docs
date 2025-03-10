---
title: Authoring Pages
---

# Authoring Pages

Basically you just hack up markdown files. Tables seem to work OK.

| Key | Value |
| :--- | :--- |
| Here | There |

In the [site setup guide](../set-up) you'll see that we add Markdown admonitions as an extension.

You'll also note that relative paths are slightly counter-intuitive. Mkdocs create html where a page is a directory with an `index.html` file inside. This means that a reference to a page in the same directory has to actually be [`../set-up`](../set-up)

!!! note "Here's a note"
    We can add notes etc. using a Markdown extension. While it might make pages a **tiny** bit less portable, it's not hard to set up and is valuable for documentation.

There's an edit button in the top right corner &nearr;

By default it offers to put the edit onto the master branch :(

---

# Have a try!

Any edits in here will be added to the published site. They won't appear immediately: the pipeline has to run
after the change is committed/merged to `main`

---

---

### Dunstan 20230717

Sample of stuff including

```
a code block
```

---
