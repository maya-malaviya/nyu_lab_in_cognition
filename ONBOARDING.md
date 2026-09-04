# Course Website Onboarding & Maintenance

This repo builds the **Lab in Cognition & Perception** course website using
[Jupyter Book](https://jupyterbook.org/). 

This guide is intended to help future course staff with best practices on updating the site, understanding the repo, and version control.

If you're starting a new semester, please fork this repository. This will make a "fresh copy" of the repo so that this (Fall 2026) version of the class website is not overridden or hosted through my (Maya's) github pages.

---

## Repository Structure & Relationship to Website 

The way the repo works is that all the files in `book/` compile to build the website using jupyter-book. This happens when you make a commit. Then, when you push your changes to the remote m GitHub actions uses the information from jupyter-book to deploy to a website hosted by GitHub pages.

As such, everything needed for the website is in `book/`. You can edit `.md` files to change what's written on the site. If you want to edit the site structures and settings for builds, you can edit [`book/_toc.yml`](book/_toc.yml) and/or [`book/_config.yml`](book/_config.yml).

<!-- Note that this site contains Jupyter notebooks (file type `.ipynb`). These are not re-run every time the site builds.  (If you edit notebook *code* and
   want fresh outputs, you must re-run the notebook yourself in Jupyter first — see
   [Editing notebooks](#editing-notebook-code).)-->

---

## Setup

### Python & Virtual Environments for Executing the Build
You will need to be able to run Python in order to preview the website. Typically, a good way to do this is setting up a Python virtual environment. Then, we will install the `jupyter-book` package which has written all of the code to make the files -> website conversion run smoothly. You don't need to know much about virtual environments persay, just make sure it's activated. (TODO: add docs for virtual envs)

To build a virtual environment, run the following commands:
```bash
conda create -n lab_in_cog python=3.11 -y
conda activate lab_in_cog
pip install "jupyter-book==1.0.3"
```

Once this is done, you should be able to preview the site. 

### GitHub Actions & Pages for Deploying the Site

GitHub disables Actions and Pages by default. To enable them, go to https://github.com/your-username/nyu_lab_in_cognition, then:

1. **Enable Actions:** click on the **Actions** tab. Then click *"I understand my workflows, go ahead and enable them."*
2. **Enable Pages:** go to **Settings → Pages → Build and deployment → Source** and
   select **"GitHub Actions"** (switch away from the default option "Deploy from a branch"). New settings will appear, but they can be ignored since the rest is handled by jupyter-book.

---

## Previewing, Editing, & Publishing

In Terminal, activate your virtual environment, then navigate to the `book` directory (if you're not in `nyu_lab_in_cognition` in Terminal, you may need to `cd` to that from wherever you are).

```bash
conda activate lab_in_cog
cd book
make book     # build the HTML into book/_build/html/
make serve    # serve it at http://localhost:8000
```

Then open **http://localhost:8000/** in a browser. After each edit, re-run `make book`
and refresh the page. (`make serve` keeps running in the background; stop it with Ctrl-C.)

<!-- > **What `make book` actually does:** it runs `jupyter-book build ./` and then copies
> several image/data folders into `_build/html/`. Those copy steps exist because some
> images live in per-chapter `images/` folders that Jupyter Book doesn't pick up on its
> own. If you add a new chapter with its own `images/` folder, you may need to add a
> matching `cp -r` line to [`book/Makefile`](book/Makefile). -->

## Publishing (deploy)

Once your local preview looks right, you can commit your change and push it to the main branch:

```bash
git add -A
git commit -m "Describe your change"
git push origin main
```

The push triggers the GitHub Actions workflow which means the site will update. Watch it at
**`https://github.com/<your-username>/nyu_lab_in_cognition/actions`**.
First build takes ~3–5 minutes. When it's green, the site is live at:

**`https://<your-username>.github.io/nyu_lab_in_cognition/`**


### Common edits
- **Change page text:** edit the relevant `.md` or `.ipynb` under `book/`.
- **Add/remove/reorder pages:** edit [`book/_toc.yml`](book/_toc.yml).
- **Change the title, logo, or JupyterHub link:** edit [`book/_config.yml`](book/_config.yml).

<!-- #### Editing notebooks

Because `execute_notebooks: off`, the site shows whatever outputs are *saved in the
`.ipynb`*. If you change code in a notebook, you must re-run it in Jupyter and save,
or the site will show stale output. To run the notebooks you need the full scientific
stack, which is what the root [`Makefile`](Makefile) `setup` target is for (it targets
JupyterHub but works locally too):

```bash
make setup            # creates conda env 'cognition' with numpy, pandas, etc.
make install-kernel   # register it as a Jupyter kernel, if needed
```

The Makefile also has testing targets to check notebooks still execute cleanly —
`make test-chapters`, `make test-notebook NB=book/labs/Foo.ipynb`, `make validate-quick`,
etc. Run `make help` to see them all. -->

---


## JupyterHub (student notebook access)

The book has a launch button (rocket icon) that opens notebooks in NYU's JupyterHub. The
Hub URL is set in [`book/_config.yml`](book/_config.yml) under `launch_buttons:
jupyterhub_url`. To set up a Hub for a new term, email
`Instructional-Tools-For-Coding@nyu.edu` (see [`README.md`](README.md) for the full
procedure), then update that URL in the config.

<!-- ---

## Forking for a new semester

When you inherit this repo for a new term, the checklist is:

1. **Fork** the repo to your own GitHub account.
2. **Enable Actions + Pages** on your fork (see [First-time deploy](#first-time-deploy-on-a-fresh-fork--required-settings)).
3. **Update [`book/_config.yml`](book/_config.yml):**
   - `title:` — the term (e.g. "Lab in C&P (Fall 2026)").
   - `repository: url:` — point it at *your* fork, not the previous maintainer's.
     This controls the GitHub/"suggest edit" buttons on the published site.
   - `launch_buttons: jupyterhub_url:` — your term's JupyterHub URL.
4. **Update [`book/intro.md`](book/intro.md)** acknowledgements if you want credit.
5. **Update the schedule/syllabus** pages under `book/` for your term's dates.
6. Set up your local env ([One-time setup](#one-time-setup)), preview, then push.

---

## Troubleshooting

| Symptom | Likely cause / fix |
|---|---|
| `make book` fails: `jupyter-book: command not found` | `conda activate lab_in_cog` first. |
| Build succeeds but pushed changes don't appear online | Pages not set to "GitHub Actions" source, or the Actions run failed — check the Actions tab. |
| Notebook shows old output on the site | `execute_notebooks: off` — re-run the notebook in Jupyter and commit the saved `.ipynb`. |
| A new chapter's images are missing | Add a `cp -r chapters/NN/images ...` line to [`book/Makefile`](book/Makefile). |
| Lots of warnings during build | Normal — this book builds with ~260 non-fatal warnings. Only a hard "build failed" matters. | -->
