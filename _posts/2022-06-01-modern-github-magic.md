---
layout: post
title: Modern Github Magic
categories: [Github, VS Code, HTML, CSS]
---

Poring through dozens of students' projects for school has introduced some interesting workflow challenges. As a developer, whenever I repeat an action, I start to hear the problem-solver in my brain whisper how I can DRY out the process.

Here are some useful tools in Github that have helped me out while I've bounced from student project to project over the last few weeks. 

1. In Github, if you're on a repository's page or any file in that repo, you can press `.` to enter an online VS Code editor window. This same online editor window can actually be synced with your account to preserve your IDE's themes, extensions, and settings! While it suffers from the same testing/live view issues as the Github Repositories extension in (2.), it's still a really nice tool to view and change giles on Github without downloading anything whatsoever. 

2. From a local (i.e. not from #1) VS Code window, you can use the Github Repositories extension to edit Github repositories directly from your IDE without ever cloning it. Nb. this is only useful for small edits since it won't play nicely when trying to run a local React app, for instance, or even a Live Preview. But if you only need to edit the contents of a repo without needing to worry about your live view, testing suite, etc., it's a fabulous tool. All the entries of this blog to date have been written through this interface. 

3. Finally, one that's been a real godsend, which is if you prepend `https://htmlpreview.github.io/?` to any `.html` file in a Github repo, you can view a live HTML version of that page. As you might expect, this will break any local references, such anything pointing to `http://localhost:xxxx`, but it's a super-fast way to view vanilla JS projects without the steps of cloning and viewing that HTML page. So cool! 