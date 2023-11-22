---
title: Redirector
description: A simple redirector to a URL entered by the user
date: 2023-01-01
categories:
  - tools
---

# Input URL into the box and click the go button

Then click the link to go to the website.

<script>
  function func() {
    var u = document.getElementById("url").value;
    if (u.indexOf("http://") == -1 && u.indexOf("https://") == -1) {
      u = "http://" + u;
    }
    var link = document.getElementById("link");
    link.href = u;
    console.log(u);
  }
</script>

<input title="URL" type="text" name="url" id="url" />
<button onclick="func()">go</button>
<a id="link" href="javascript:alert('Empty Input')">
  click here to go to your website</a
>
