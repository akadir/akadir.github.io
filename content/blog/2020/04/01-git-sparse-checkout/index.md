---
title: 'git sparse checkout'
date: "2020-04-20T22:19:32.169Z"
description: 'What if we only want to fetch only specific part of the remote git repository?'
---

Today I have learned that there is an option to fetch only specific folders or files from remote git repository. Yes, you are right, it seems like the opposite of .gitignore.

So, how do we use this?

Let's say we will fetch remote repository that structured as shown in below:

```sh
.
├── folder-we-want
│   ├── file-1.jar
│   └── file-2.jar
├── folder-we-do-not-want
│   ├── other-file.python
│   └── another-file.cpp
└── README.md
```

But we only want to fetch `folder-we-want`'s content and `README.md` file. To do that, first create a directory to fetch this imaginary repository and run `git init` and add our remote url.

```sh
mkdir imaginary-repo
cd imaginary-repo
git init
git remote add –f <imaginary-repo-url>
```

After that we need to enable sparse checkout using this command:

```sh
git config core.sparsecheckout true
```

Now we can add folder path we do not want in our local repository into `.git/info/sparse-checkout`. Our updated`.git/info/sparse-checkout` file content should be as below:

```sh
folder-we-do-not-want
```

<i>Note: in git-sparse-checkout documentation it is stated that, by default, sparse-checkout file uses the same syntax as .gitignore files.</i>


After all these modifications, when we fetch the repository with `git pull <remote> <branch>` command our local directory(`imaginary-repo`)'s structure should be as below:

```sh
├── folder-we-want
│   ├── file-1.jar
│   └── file-2.jar
└── README.md
```

Thanks to sparse checkout, we do not have the folder named `folder-we-do-not-want`.

We also can use sparse-checkout in existing local repositories as below. The only difference, after enabling sparse checkout and updating the `.git/info/sparse-checkout`file, we should run this command: 

```sh
git config core.sparsecheckout true
echo folder-we-do-not-want >> .git/info/sparse-checkout
echo another/unnecessary/folder >> .git/info/sparse-checkout
git read-tree -mu HEAD
```

---

<i>You can read detailed documentation from here: 
<a href="https://git-scm.com/docs/git-sparse-checkout" target="_blank">git-sparse-checkout</a>
and more detailed and experimental blog post from here: <a href="https://github.blog/2020-01-17-bring-your-monorepo-down-to-size-with-sparse-checkout/" target="_blank">Bring your monorepo down to size with sparse-checkout</a></i>

