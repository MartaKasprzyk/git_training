**Co zrobić aby usunąć pliki idea ze zdalnego repozytorium po dodaniu pliku gitignore w istniejącym już repozytorium?**

```
git rm -r --cached .idea
git commit -m "Removing .idea from repository"
git push
```

**Po wprowadzeniu zmian w pliku pom na gałęzi main, próba zmiany gałęzi z main na test.**
**Jakie pojawiły się problemy jak można je rozwiązać? Opisz dwa sposoby.**

Po próbie przełączenia się z gałęzi, na której zostały wprowadzone zmiany pojawia się komunikat:
```
PS C:\Users\markaspr\git_training> git checkout test
error: Your local changes to the following files would be overwritten by checkout:
pom.xml
Please commit your changes or stash them before you switch branches.
Aborting
```
Rozwiązanie 1 
Dodanie plików, w których zostały wprowadzone zmiany do indexu oraz ich zacommitowanie.
`git add <modified_files>`
`git commit -m "info about changed files"`

Rozwiązanie 2
"Odłożenie" zmian, tymczasowe ich zapisane bez commitowania. 
`git stash`
Zmiany mogą być następnie zaaplikowane ponownie poprzez `git stash apply`.

**Opisać jaka jest różnica między git checkout HEAD~3 i git reset --hard HEAD~3.**

`git checkout HEAD~3` - przełączenie się na wersję sprzed 3 commitów, 
miejsce, na które wskazuje HEAD będzie przesunięte o 3 commity do tyłu  
i folder roboczy, na którym pracujemy będzie odzwiercidlał stan sprzed 3 commitów

`git reset --hard HEAD~3` - index oraz folder roboczy, na którym pracujemy zostaną przywrócone do stanu sprzed 3 commitów

`reset --hard` zresetuje folder roboczy, wyrzucając wybrane zmiany z historii, 
natomiast checkout nie ma wpływu na historię i jedynie wyświetli nam stan folderu roboczego w wybranym punkcie w historii zmian

**Opisać różnicę między revert i reset.**

`git revert <commit>` - tworzy nowy commit, który odwraca zmiany z wybranego <commita>, nie modyfikuje historii zmian
natomiast
`git reset <commit>` - modyfikuje historię zmian i przesuwa HEAD do wybranego commita

`reset --soft` - przesuwa HEAD do wybranego commita, ale nie modyfikuje index i folderu roboczego
`reset --mixed` - przesuwa HEAD do wybranego commita, modyfikuje (resetuje) index, ale nie folder roboczy
`reset --hard` - przesuwa HEAD do wybranego commita, modyfikuje (resetuje) index oraz folder roboczy, 
commity następujące po wybranym punkcie są usuwane

test revert:
```
PS C:\Users\markaspr\git_training> git log
commit 1f901bc6af10a8393d1e631baf1988654661e91e (HEAD -> test)
Author: Marta Kasprzyk <marta.kasprzyk@capgemini.com>
Date:   Mon Sep 23 10:49:07 2024 +0200

    Revert "change1"

    This reverts commit 8e92060e3bb586385c3dfe82b4daa15722547e5e.

commit 8e92060e3bb586385c3dfe82b4daa15722547e5e
Author: Marta Kasprzyk <marta.kasprzyk@capgemini.com>
Date:   Mon Sep 23 10:47:22 2024 +0200

    change1

commit d1c43ea097b47d6d4c56a125eee5c12f6157fdab
Author: Marta Kasprzyk <marta.kasprzyk@capgemini.com>
Date:   Fri Sep 20 16:49:21 2024 +0200

    added developers section to pom file

test reset --hard:
na potrzeby testu przywróciłam zmiany odwrócone przez git revert

PS C:\Users\markaspr\git_training> git log
commit 78a9e2b7eec99c0fa8abdc20e7f9d39f0df284bd (HEAD -> test)
Author: Marta Kasprzyk <marta.kasprzyk@capgemini.com>
Date:   Mon Sep 23 10:52:25 2024 +0200

    Reapply "change1"

    This reverts commit 1f901bc6af10a8393d1e631baf1988654661e91e
```
i wykonałam git reset --hard
```
PS C:\Users\markaspr\git_training> git reset --hard 1f901bc6af10a8393d1e631baf1988654661e91e
HEAD is now at 1f901bc Revert "change1"
PS C:\Users\markaspr\git_training> git log
commit 1f901bc6af10a8393d1e631baf1988654661e91e (HEAD -> test)
Author: Marta Kasprzyk <marta.kasprzyk@capgemini.com>
Date:   Mon Sep 23 10:49:07 2024 +0200

    Revert "change1"

    This reverts commit 8e92060e3bb586385c3dfe82b4daa15722547e5e.
```

**Uruchomić komendę git clean -n przeanalizować co stało by się gdby nie było -n.**
```
PS C:\Users\markaspr\git_training> git status
On branch test
Untracked files:
(use "git add <file>..." to include in what will be committed)
test.md

nothing added to commit but untracked files present (use "git add" to track)
PS C:\Users\markaspr\git_training> git clean -n
Would remove test.md
```
git clean -n - pokazuje pliki, które nie są trackowane i mogą zostać usunięte przez git clean -f

**git remote prune -n origin przeanalizować co stało by się gdby nie było -n**

`git remote prune` - czyści nieaktualne odniesienia do zdalnych gałęzi, 
które już nie istnieją w zdalnym repozytorium 

`git remote prune -n` - uruchomienie "na sucho", oflagowana komenda pokaże, co zostałoby usunięte

**Jak usunąć nie używanego brancha (lokalnie i zdalnie)?**

lokalnie:
`git branch -d <branch_name>` - jeśli gałąź została zmergowana
`git branch -D <branch_name>` - jeśli gałąź nie została zmergowana

zdalnie:
`git push origin --d <branch_name>`

**Co robi opcja --nocommit?**

`git cherry-pick <commit> --no-commit` - komenda dodaje zmiany z <commit> na wybranej gałęzi bez automatycznego tworzenia commita, 
opcja ta orzydaje się, żeby podejrzeć lub wprowadzić odatkowe modyfikacje do zmiany z <commit>, którą chcemy wprowadzić













