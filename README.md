# Introduction to Git and GitHub

Welcome! Today's assignment will walk you through using Git, GitHub, RStudio to interact with both, and, along the way, practice using Quarto and RStudio. Cool! The content has been adapted from a [workshop](https://rstudio-conf-2020.github.io/r-for-excel/github.html) that Openscapes developed.

Please work with a partner on one computer and take turns "driving" (touching the keyboard) vs. "navigating" (figuring out what to do). This is called pair programming and is common practice in industry and computer science.

If you need to jump to a different part of this document, click the menu icon (<img src="https://github.com/ds4eeb/GitAndGitHub/blob/main/data/images/menuicon.png" width="35"/>) in the upper right of this document to see a simple table of contents.

## 1. Create your repository
The first step is to create the assignment repository in your personal GitHub account so that you can work on it. You will use the ds4eeb repository as a template.

To do that, click the green "Use this template" button in the upper right and select "Create in a new repository."  
<p align="center">
<img src="https://github.com/ds4eeb/GitAndGitHub/blob/main/data/images/usetemplate.jpg" width="400" />
</p>

Please start the name of your repository with "GitAndGitHub" so that we can identify it later, and choose Public for Visibility.

Admire your first repo! It should look a lot like the ds4eeb one you started with, but now it's in your own account. You can see that in the URL (web address) and in the upper left of the GitHub page, where it shows the repo name after your account name.
<p align="center">
<img src="https://github.com/ds4eeb/GitAndGitHub/blob/main/data/images/ownrepo.png" width="400" />
</p>

## 2. Set up RStudio to communicate with GitHub

### 2.1 Install packages
We're going to use a "package" called `usethis` to make it easy for RStudio and GitHub to work together. In R, a package bundles together reusable code to accomplish a particular purpose, along with data and documentation, and is easy to share with others. Packages are part of what makes R so powerful because anyone can improve or add to existing base R capabilities.

The traditional place to download packages is from CRAN, the Comprehensive R Archive Network, which is the same place you downloaded R. As Julie Lowndes has said, CRAN is like a grocery store or iTunes for vetted R packages.

Open RStudio on your computer and type this in the console:  
```
install.packages("usethis")
```

You should see R respond by saying it is trying and then downloaded the package. Now that you have the package, you shouldn't need to download this package again. 

Now you need to activate (or "attached") the package to your current session in R. You'll need to do this each time you want to use the package in a new R session. Type this in the console:  
```
library("usethis")
```

When `usethis` is successfully attached, you won’t get any feedback in the Console. So unless you get an error, this worked for you.

Now let’s do the same with the `here` package, which makes it easy to work with file paths in RStudio projects:
```
install.packages("here")
library("here")
```

`here` is a “chatty” package and, when attached, responds with the filepath we are working from.

### 2.2 Configure Git in RStudio
This next set up step is a one-time thing! You will only have to do this once per computer.

You will need to know your GitHub username, the email address you created your GitHub account with, and your GitHub password.

We will be using the use_git_config() function from the usethis package we just installed. Since we already installed and attached this package, type the following into your Console, but replace USERNAME with your GitHub username, and replace EMAIL with the same email you used to create your GitHub account:

```
use_git_config(user.name = "USERNAME", user.email = "EMAIL")
```

If you see `Error in use_git_config() : could not find function "use_git_config"`, please run `library("usethis")`

You can check that things worked and diagnose problems by typing this in the console:
```
# check by running a git situation-report: 
#   - your user.name and user.email should appear in global Git config 
git_sitrep()
```

### 2.3 Get a personal access token (PAT)
Personal access tokens are an alternative to using passwords for authentication to GitHub, and they make working with GitHub through RStudio much easier. I recommend creating one for this class, though you could use an existing one if you have it. The instructionsn below are modified from [this longer guide](https://usethis.r-lib.org/articles/git-credentials.html) from the creators of the usethis package. 

Type:
```
usethis::create_github_token()
```

As a small note, see how we referenced the `usethis` package with two colons `::` before calling the `create_github_token()` function? That's a way to be very specific to R about which package to look in for the function. It's not usually necessary if you have the package loaded.

Assuming you’re signed into GitHub (you should be), `create_github_token()` takes you to a pre-filled form to create a new PAT. You can get to the same page in the browser by clicking on “Generate new token” from https://github.com/settings/tokens. The advantage of create_github_token() is that the creators of the usethis package have pre-selected some recommended scopes, which you can look over and adjust before clicking “Generate token”.

It is a very good idea to describe the token’s purpose in the Note field, because one day you might have multiple PATs. We recommend naming each token after its use case, such as the computer or project you are using it for, e.g. “2026 ds4eeb personal laptop”. In the future, you will find yourself staring at this list of tokens, because inevitably you’ll need to re-generate or delete one of them. Make it easy to figure out which token you need to fiddle with.

GitHub encourages the use of perishable tokens, with a default Expiration period of 30 days. Unless you have a specific reason to fight this, I recommend accepting this default. I assume that GitHub’s security folks have good reasons for their recommendation. But, of course, you can adjust the Expiration behaviour as you see fit (perhaps the end of this course).

Once you’re happy with the token’s Note, Expiration, and Scopes, click “Generate token”.

You won’t be able to see this token again, so don’t close or navigate away from this browser window until you store the PAT locally. Copy the PAT to the clipboard, anticipating what we’ll do next: trigger a prompt that lets us store the PAT in the Git credential store.

Sidebar about storing your PAT: If you use a password management app, such as 1Password or Bitwarden (highly recommended!), you might want to add this PAT (and its Note) to the entry for GitHub. Storing your PAT in the Git credential store is a semi-persistent convenience, sort of like a browser cache or “remember me” on a website, but it’s quite possible you will need to re-enter your PAT in the future. You could decide to embrace the impermanence of your PAT and, if it is somehow removed from the store, you’ll just re-generate a new PAT and re-enter it. If you accept the default 30-day expiration period, this is a workflow you’ll be using often anyway. But if you create long-lasting tokens or want to play around with the functions for setting or clearing your Git credentials, it can be handy to have your own record of your PAT in a secure place, like 1Password or Bitwarden.

### 2.4 Put your PAT into the local Git credential store
We assume you’ve created a PAT and have it available on your clipboard.

How to insert your PAT in the Git credential store? Type this in the RStudio console:
```
gitcreds::gitcreds_set()
```

You already have the `gitcreds` package installed, because it came with `usethis`.

If you don’t have a PAT stored already, it will prompt you to enter your PAT. Paste!

```
? Enter password or token: ghp_xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
-> Adding new credentials...
-> Removing credentials from cache...
-> Done.
```

If you already have a stored credential, `gitcreds::gitcreds_set()` reveals this and will even let you inspect it. This helps you decide whether to keep the existing credential or replace it. When in doubt, embrace a new, known-to-be-good credential over an old one, of uncertain origins.

If you have worked with GitHub before and previously made your GitHub PAT available by setting the `GITHUB_PAT` environment variable in `.Renviron`, come talk to the instructor for helping addressing it.

The general function `usethis::git_sitrep()` will report on your PAT, along with other aspects of your Git/GitHub setup.
```
git_sitrep()
```

### 2.5 A note about ongoing PAT maintenance
You are going to be (re-)generating and (re-)storing your PAT on a schedule dictated by its expiration period. By default, once per month.

When the PAT expires, return to https://github.com/settings/tokens and click on its Note. (You do label your tokens nicely by use case, right? Right?) At this point, you can optionally adjust scopes and then click “Regenerate token”. You can optionally modify its Expiration and then click “Regenerate token” (again). As before, copy the PAT to the clipboard, call `gitcreds::gitcreds_set()`, and paste!

Hopefully it’s becoming clear why each token’s Note is so important. The actual token may be changing, e.g., once a month, but its use case (and scopes) are much more persistent and stable.

Phew! Glad we have now proved to GitHub who we are when we use RStudio.

### 2.6 Ensure that Git/GitHub/RStudio are communicating
We are going to go through a couple steps to make sure the Git/GitHub are communicating with RStudio.

First, create a new project by clicking on the drop-down menu in the upper right and selecting "New Project". Alternatively, you could also go to File > New Project…, or click the little white + in a green circle with the R box in the top left.
<p align="center">
<img src="https://github.com/ds4eeb/GitAndGitHub/blob/main/data/images/newproject1.png" width="600" />
</p>

Next, select Version Control:
<p align="center">
<img src="https://github.com/ds4eeb/GitAndGitHub/blob/main/data/images/newproject2.png" width="400" />
</p>

Then select Git (since we are using Git):
<p align="center">
<img src="https://github.com/ds4eeb/GitAndGitHub/blob/main/data/images/newproject3.png" width="400" />
</p>

Do you see this?
<p align="center">
<img src="https://github.com/ds4eeb/GitAndGitHub/blob/main/data/images/newproject4.png" width="400" />
</p>

If yes, hooray! If no, we will help you troubleshoot.

1. Double check that your GitHub username and email are correct
2. Troubleshooting, starting with [HappyGitWithR’s troubleshooting chapter](https://happygitwithr.com/troubleshooting.html)

* `which git` (Mac, Linux, or anything running a bash shell)
* `where git` (Windows, when not in a bash shell)

## 3. Clone your repository using RStudio
Let’s clone your repo on GitHub to your local computer using RStudio. Unlike downloading, cloning keeps all the version control and user information bundled with the files.

### 3.1 Copy the repo address
First, copy the web address of the repository you want to clone. We will use HTTPS.

<p align="center">
<img src="https://github.com/ds4eeb/GitAndGitHub/blob/main/data/images/gh_repo_clone1.png" width="600" />
</p>

### 3.2 Paste the repo address
Paste the repo address (which is still in your clipboard) into in the “Repository URL” field in RStudio, where you should still have the New Project wizard in progress. The “Project directory name” should autofill; if it does not press tab, or type it in. It is best practice to keep the “Project directory name” THE SAME as the repository name.

When cloned, this repository is going to become a folder on your computer.

At this point you can save this repo anywhere. There are different schools of thought, but we think it is useful to create a high-level folder for this class where you will keep your github repos to keep them organized. Press “Browse…” to navigate to a folder and you have the option of creating a new folder.

Finally, click Create Project.

<p align="center">
<img src="https://github.com/ds4eeb/GitAndGitHub/blob/main/data/images/gh_repo_clone2.png" width="600" />
</p>

### 3.3 Admire your local repo
If everything went well, the repository will show up in RStudio!

<p align="center">
<img src="https://github.com/ds4eeb/GitAndGitHub/blob/main/data/images/gh_repo_clone3.png" width="600" />
</p>

The repository is also saved to the location you specified, and you can navigate to it as you normally would in Finder or Windows Explorer:
<p align="center">
<img src="https://github.com/ds4eeb/GitAndGitHub/blob/main/data/images/gh_repo_clone4.png" width="600" />
</p>

Hooray!

### 3.4 Inspect your local repo
Let’s notice a few things:

First, our working directory is set to the location of the repo and the Project (they are both in the same place), and GitAndGitHub is also named in the top right hand corner.

Second, we have a Git tab in the top right pane! Let’s click on it.
<p align="center">
<img src="https://github.com/ds4eeb/GitAndGitHub/blob/main/data/images/gh_repo_clone5.png" width="600" />
</p>

Our Git tab has one item, an `.Rproj` file.

This has been added to our repo by RStudio — we can also see it in the File pane in the bottom right of RStudio. This is a helper file that RStudio has added to streamline our workflow with GitHub and R. Note that this files begins with a period (`.`), which means they are hidden files: they show up in the Files pane of RStudio but won’t show up in your Finder or Windows Explorer.

Going back to the Git tab, both these files have little yellow icons with question marks `?`. This is GitHub’s way of saying: “I am responsible for tracking everything that happens in this repo, but I’m not sure what is going on with these files yet. Do you want me to track them too?”

We will handle this in a moment; first let’s look at the README.md file.

## 4. Edit a Quarto-like document
The `README.md` file (which you have been following as instructions, by the way) is a GitHub-flavored Markdown file, which is very similar to Quarto. It’s like a Quarto file without the abilities to run R code. It does, however, have the super-power of being displayed easily on GitHub, which made these instructions easy to read.

You will edit the file for practice reading and interpreting Quarto, and then illustrate how GitHub tracks files that have been modified (to complement seeing how it tracks files that have been added).

README files are common in programming; they are the first place that someone will look to see why code exists and how to run it.

Open the README by making sure the Files pane is selected in the lower right, then clicking the README.md file. It will open in the Scripts pane (upper left).
<p align="center">
<img src="https://github.com/ds4eeb/GitAndGitHub/blob/main/data/images/edit1.png" width="600" />
</p>

Skim through it to see which Quarto tags you recognize. Look for headers and lists!

In your README, find the Header. It should begin with a hashtag `#`.

Please edit it to include your name and your partner's name. Make sure you keep the `#` so that it still displays as a header! Something like:

`# Malin's GitAndGitHub repo`

Save this file (Cmd/Ctrl-S or File -> Save) and notice how it shows up in your Git tab. It now has a blue “M”: GitHub is already tracking this file line-by-line, so it knows that something is different: it’s Modified with an M.

Great! You just made your first edits to a Git repository. Now let’s sync these changes back to GitHub.

## 5. Sync from RStudio (local) to GitHub (remote)
Syncing to GitHub.com means 4 steps:

1. Pull
1. Stage
1. Commit
1. Push

We get ready by clicking on the Commit section.
<p align="center">
<img src="https://github.com/ds4eeb/GitAndGitHub/blob/main/data/images/sync1.png" width="600" />
</p>

### 5.1 Pull
We start off by “Pulling” from the remote repository (GitHub.com) to make sure that our local copy has the most up-to-date information that is available online. Right now, since we just created the repo and are the only ones that have permission to work on it, we can be pretty confident that there isn’t new information available. But we pull anyways because this is a good habit to get into for when you start collaborating with yourself across computers or others. Best practice is to pull often: it costs nothing (other than an internet connection).

Pull by clicking the teal Down Arrow. You should get a notification that your local copy is already up to date.

### 5.2 Stage
Notice also how when you highlight a filename, a preview of the differences displays below:
<p align="center">
<img src="https://github.com/ds4eeb/GitAndGitHub/blob/main/data/images/sync2.png" width="400" />
</p>

Next, click the boxes next to each file. This is called “staging" a file: you are indicating that you want GitHub to track the new changes to this file, and that you will be committing it shortly. Notice:

* `.Rproj` file: the question marks turn into an A because this is a new file that has been added to your repo (automatically by RStudio, not by you).
* `README.md` file: the M indicates that this was modified (by you)

These are the codes used to describe how the files are changed (from the [RStudio cheatsheet](https://rstudio.github.io/cheatsheets/html/rstudio-ide.html)):
<p align="center">
<img src="https://github.com/ds4eeb/GitAndGitHub/blob/main/data/images/sync3.png" width="200" />
</p>

### 5.3 Commit
Committing is different from saving our files to our local harddrive (which we still have to do! RStudio will indicate a file is unsaved with red text and an asterix). We commit a single file or a group of files when we are ready to save a snapshot in time of the progress we’ve made. Maybe this is after a big part of the analysis was done, or when you’re done working for the day.

Committing our files is a 2-step process.

First, you write a “commit message,” which is a human-readable note about what has changed that will accompany GitHub’s non-human-readable alphanumeric code to track our files. I think of commit messages like breadcrumbs to my Future Self: how can I use this space to be useful for me if I’m trying to retrace my steps (and perhaps in a panic?).

Second, you press Commit.
<p align="center">
<img src="https://github.com/ds4eeb/GitAndGitHub/blob/main/data/images/sync4.png" width="400" />
</p>

When we have committed successfully, we get a rather unsuccessful-looking pop-up message. You can read this message as “Congratulations! You’ve successfully committed 2 files, 1 of which is new!” It is also providing you with that alphanumeric SHA code that GitHub is using to track these files.

If our attempt was not successful, we will see an Error. Otherwise, interpret this message as a joyous one.

> Does your pop-up message say “Aborting commit due to empty commit message.”
> GitHub is really serious about writing human-readable commit messages.

<p align="center">
<img src="https://github.com/ds4eeb/GitAndGitHub/blob/main/data/images/sync5.png" width="400" />
</p>

When we close this window there is going to be (in my opinion) a very subtle indication that we are not done with the syncing process.

<p align="center">
<img src="https://github.com/ds4eeb/GitAndGitHub/blob/main/data/images/sync6.png" width="400" />
</p>

We have successfully committed our work as a breadcrumb-message-approved snapshot in time, but it still only exists locally on our computer. We can commit without an internet connection. However, we have not done anything yet to tell GitHub that we want this pushed to the remote repo at GitHub.com. So as the last step, we push.

### 5.4 Push
The last step in the syncing process is to Push! Click the green up arrow that says "Push."

Awesome! We’re done here in RStudio for the moment, let’s check out the remote repo on github.com.

## 6 Commit history
The files you modified should be on github.com on the page for your repo. Go check them out!

Notice how the `README.md` file you edited is automatically displayed at the bottom. Since it is good practice to have a README file that identifies what code does (i.e. why it exists), GitHub will display a Markdown file called README nicely formatted.

Let’s also explore the commit history by clicking where it says "Commits":
<p align="center">
<img src="https://github.com/ds4eeb/GitAndGitHub/blob/main/data/images/history1.png" width="700" />
</p>

The 2 commits you’ve made (the first was when we originally initiated the repo from GitHub.com) are there! (my screenshot has three because I made another commit by mistake in there).
<p align="center">
<img src="https://github.com/ds4eeb/GitAndGitHub/blob/main/data/images/history2.png" width="700" />
</p>

## 7 Make your own Quarto document

### 7.1 Create and edit
Now time to make your own Quarto document!

Select File > New File > Quarto Document … (or click the green plus in the top left corner and select "Quarto Document").

RStudio will give you a dialog box to fill in the title and author now. Fill these in however you wish and leave the other options at their defaults.

RStudio provides you with some default contents in the Quarto document, including a YAML header, some text, and some code blocks. 

Please delete the text and code blocks (everything below the second set of `---`).

> __Efficiency Tip__: I use Cmnd-Shift-Down Arrow (Mac) or Ctrl-Shift-End (Windows) to highlight text from my cursor to the end of the document.

Now please edit a few things in the Quarto document:

1. Add a date to the YAML header (e.g., `date: 2025-01-02`)
2. Edit the YAML header output format to use gfm (instead of html)
3. Add a least two headers of different priorities (different numbers of #s)
4. Add at least one line of regular text with at least one word each in italics and bold
5. Add one code block in R that adds 10+20

The [Quarto basics page](https://quarto.org/docs/authoring/markdown-basics.html) is a useful reference!

Notice that RStudio is displaying the name of your Quarto document in red? That means it has unsaved chnages. Now would be a good time to save them!

### 7.2 Render
To render your Quarto document, click the "->Render" button at the top of this window.
<p align="center">
<img src="https://github.com/ds4eeb/GitAndGitHub/blob/main/data/images/render1.png" width="400" />
</p>

You should see RStudio running for a bit and display some messages in the bottom-left pane. 

When it's done, you'll see the rendered document in the Viewer pane (right side). Exciting!

If you instead run into error messages, see if you can debug where the error is. Or call someone from the teaching team over to help!

Now also look at the Files pane in the lower right (you may need to click on the Files tab over there to show it).
<p align="center">
<img src="https://github.com/ds4eeb/GitAndGitHub/blob/main/data/images/render2.png" width="400" />
</p>

Notice that a new .md file has also been created. This is the rendered Github-flavored markdown file that you specified your .qmd file would be output as. Cool!

If you want, play around with other output formats, like .pdf, .docx, or .html.

### 7.3 Sync
Remember what we did in Section 5? Now time to do it on your own: 
1. Pull
2. Stage (be sure to stage both the .qmd file and the rendered .md file that you created)
3. Commit
4. Push

### 7.4 View on GitHub
To make sure it all worked, now go to your repo on github.com and find the new .md file you just pushed to GitHub. Click to open it. You might need to refresh your repo's webpage for it to show up.

Notice how it is rendered nicely and formats well on GitHub. Cool!

## 8 Create an issue on GitHub
As your last task, you'll create a new Issue on GitHub. 

To get started, click the "Issues" tab at the top of the GitHub page.

Now, click the big green "New Issue" button in the upper right.

Think of a challenge you had during this assignment (whether you solved it or not) and type a short but descriptive title.

Then, in the body, describe the issue you had and whether and how you addressed it.

If you want, you can play with adding Assignees (who should address this issue?) or Labels using the gear icons on the right side of the page.

Finally, click the big green "Create" button at the bottom.

Congratulations, you've submitted your first issue!

## 9 Submit your assignment
To submit this assignment, please go back to Canvas and follow the instructions on submitting the repo through Gradescope using the GitHub submission method.
