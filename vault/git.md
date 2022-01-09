---
id: esZXrAjmKkTyLkVG5Iyeb
title: Git
desc: ''
updated: 1641744531441
created: 1641739397629
---

## Change past commit author

### Interactive rebase

Interactive rebase off of a point earlier in the history than the commit you need to modify (`git rebase -i <earliercommit>`). In the list of commits being rebased, change the text from `pick` to `edit` next to the hash of the one you want to modify. Then when git prompts you to change the commit, use this:

    git commit --amend --author="Author Name <email@address.com>" --no-edit

For example, if your commit history is `A-B-C-D-E-F` with `F` as `HEAD`, and you want to change the author of `C` and `D`, then you would...

 1. Specify `git rebase -i B` ([here is an example of what you will see after executing the `git rebase -i B` command](https://help.github.com/articles/about-git-rebase/#an-example-of-using-git-rebase))
 * if you need to edit `A`, use `git rebase -i --root`
 2. Change the lines for both `C` and `D` from `pick` to `edit`
 3. Exit the editor (for vim, this would be pressing Esc and then typing `:wq`).
 3. Once the rebase started, it would first pause at `C`
 4. You would `git commit --amend --author="Author Name <email@address.com>"`
 5. Then `git rebase --continue`
 6. It would pause again at `D`
 7. Then you would `git commit --amend --author="Author Name <email@address.com>"` again
 8. `git rebase --continue`
 9. The rebase would complete.
 10. Use `git push -f` to update your origin with the updated commits.


```bash
# https://stackoverflow.com/questions/3042437/how-to-change-the-commit-author-for-one-specific-commit
git log # To find the commit hash - 1
git rebase -i <hash>
# change pick to edit
git commit --amend --author="Jonathan Scherrer <jonathan.s.scherrer@gmail.com>"
git rebase --continue
git push -f
```

### Replace

The [accepted answer][1] to this question is a wonderfully clever use of interactive rebase, but it unfortunately exhibits conflicts if the commit we are trying to change the author of used to be on a branch which was subsequently merged in. More generally, it does not work when handling messy histories.

Since I am apprehensive about running scripts which depend on setting and unsetting environment variables to rewrite git history, I am writing a new answer based on [this post](https://help.github.com/articles/changing-author-info/) which is similar to [this answer](https://stackoverflow.com/a/3404304/391161) but is more complete.

The following is tested and working, unlike the linked answer.
Assume for clarity of exposition that `03f482d6` is the commit whose author we are trying to replace, and `42627abe` is the commit with the new author. 

1. Checkout the commit we are trying to modify. 
      
        git checkout 03f482d6
2. Make the author change.

        git commit --amend --author "New Author Name <New Author Email>"

  Now we have a new commit with hash assumed to be `42627abe`.

3. Checkout the original branch.

4. Replace the old commit with the new one locally.

        git replace 03f482d6 42627abe

5. Rewrite all future commits based on the replacement.

        git filter-branch -- --all

6. Remove the replacement for cleanliness.

        git replace -d 03f482d6

7. Push the new history (only use --force if the below fails, and only after sanity checking with `git log` and/or `git diff`).

        git push --force-with-lease

Instead of 4-5 you can just rebase onto new commit:

    git rebase -i 42627abe

[1]: https://stackoverflow.com/a/3042512/3357935