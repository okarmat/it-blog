---
layout: post
title:  "GIT - Add commit push"
date:   2021-03-07 14:50:00 +0100
categories: git bash commit
---
Jeśli chcemy zapisać w repozytorium zmiany które wprowadziliśmy w plikach to należy przy pomocy git basha wykonać trzy komendy: git add, commit oraz push. Należy pamiętać, aby katalog na który wskazuje terminal był właściwy.
Więcej informacji można znaleźć [tutaj][git-add-commit-push].

{% highlight nix %}
git add .
git commit -m "opis wprowadzonej zmiany"
git push -u origin master 
{% endhighlight %}

[git-add-commit-push]: https://panjeh.medium.com/makefile-git-add-commit-push-github-all-in-one-command-9dcf76220f48
