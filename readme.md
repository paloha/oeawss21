# OEAW Summer School on AI 2021
13.09 - 17.09. 2021 in Vienna, Austria, [official website here](https://www.oeaw.ac.at/isf/forschung/fachbereiche-teams/maschinelles-lernen-ml-team/summer-school-2021).

## Tuesday lecture session

**Date**: 14.09.2021, 09:00 - 17:30 \
**Tutor**: Pavol Harar [(orcid)](https://orcid.org/0000-0001-5206-1794) \
**Assistent**: Leon Gerard [(orcid)](https://orcid.org/0000-0002-3314-396X) \
**Title**: A tool a day keeps the bad review away \
**Alt-title**: Planning, execution, and dissemination of a machine learning project \
**Slides**: [`presentation.pdf`](presentation.pdf) \

## Tuesday exercise session

The main aim of the following exercises is to have a hands on experience with some of the tools presented during the lecture session. All the commands bellow are suited for Ubuntu operating system but links to relevant resources are provided for users of other operating systems.

----

### Exercise 1 - Jupyter in Virtualenv

* Make a `volume` folder.
* Inside the `volume` folder, make a `project` folder which will be the root of your project. Do the rest of the steps in the root of the project.
* Download the `Solution_Exercise_6.ipynb` from Monday session.
* Create `requirements.txt`.
* Install & create a virtualenv in `.venv` folder.
* Install dependencies from `requirements.txt`. (The torch+cpu package is not available for Mac users, so in case you have a Mac, remove the first line of the `requirements.txt` where the `-f` flag is specified and adjust the lines of torch and torchvision to not mention cpu. The torch+cpu package should then later be used within the docker based on Ubuntu and also on Binder and Heroku. Torch which supports both cpu and gpu computation is quite large ~1GB and Heroku would complain about the size quota. You can alternatively keep two version of the file, e.g. 1. `requirements.txt` and 2. `requirements-mac.txt`)
* Run the `jupyter notebook` server.
* Run the cell 1 to see whether all imports work.
* If you run cells 1-4 the FashionMNIST data will be downloaded into `volume > data` folder which is needed only when you want to train the network. In the following exercises we will only need the pretrained models from Monday.
* Create a readme.md file with just a name of the project.

----

### Exercise 2 - Git
#### Make sure you can authenticate with git using ssh key

* Create an account on github.com.
* Go to `github.com > settings > SSH and GPG keys`.
* Copy your public ssh key from the output of `cat ~/.ssh/id_rsa.pub`.
* Add it to the github keys and verify with `ssh -T git@github.com`.
* In case of different OS or some problems consult the [github guide](https://docs.github.com/en/github/authenticating-to-github/connecting-to-github-with-ssh/testing-your-ssh-connection).


#### Create a new repository, initialize & push your project to origin

* Go to github.com and create a new repository `oeawss21`.
* Do not check the automatic creation of readme, license or other files.
* Go to your `project` folder.
* Create `.gitignore` file with `.venv`, `.ipynb_checkpoints` in it.
* Run `git init`.
* Run `git remote add origin git@github.com:<your_username>/oeawss21.git`.
* If you use `git` for the first time, you might be asked to configure your user name and commit email address with:
  * Run `git config --global user.name "Your Name"`.
  * Run `git config --global user.email "Your Name"`.
* Run `git add .`.
* Run `git commit -m "Initial commit"`.
* Run `git push origin master`.
* Now your changes should be visible in your repository on github.com.
* In case github or you changed your HEAD branch from `master` to `main`, change the commands accordingly to avoid problems.

#### More in-depth but still absolutely simple guide on git:

* https://rogerdudler.github.io/git-guide/

----

### Exercise 3 - Docker
#### Install Docker

* If you use different OS than Ubuntu, check [docker installation guide](https://docs.docker.com/engine/install/).
* On Ubuntu install with `sudo apt install docker.io`
* Check if it is installed correctly with `sudo docker run hello-world`

#### Pull Ubuntu minimal docker image

* Run `sudo docker pull intelliseqngs/ubuntu-minimal-20.04:3.0.5`.
* Add a file `.dockerfile` into your project. (Here we use a nonstandard name for a reason that we actually do not want Binder and Heroku to use our Dockerfile.)
* Base your `.dockerfile` on `intelliseqngs/ubuntu-minimal-20.04:3.0.5`.
* Fill the `.dockerfile` with commands to copy and install your project.
* Reference on writing the docker file is [here](https://docs.docker.com/engine/reference/builder/).
* In case you have problems, consult the solution in [.dockerfile](.dockerfile).

#### Build the image

* Run `sudo docker build -t oeawss21:latest -f .dockerfile .`.
* In case your Docker errors on "killed" Adjust Docker Preferences Resources RAM - make it bigger, i.e. 4 or 6GB in the settings of your Docker.
* Run `sudo docker run --rm -v <path_on_host>/volume/data:/home/volume/data -p 8888:8888 oeawss21:latest jupyter notebook --allow-root --ip 0.0.0.0`
  * `-p` forwards port 8888 of the container to 8888 on the host
  * `-v` mounts specified host folder to container's folder (paths must be absolute)
  * `--allow-root` since all in the container runs as root
  * `--ip 0.0.0.0` expose the jupyter server so host can see it

* Visit `localhost:8888` in your browser and copy the token, the jupyter notebook should now run.

----

### Exercise 4 - Weights and Biases

* Create an account at wandb.ai.
* Log in to your account and try the Example (wandb.me/intro) and run it until the "Run experiment" cell finishes.
* Check the results in the wandb.ai account.
* Check also these examples https://github.com/wandb/examples.
* If you feel motivated, change the `Solution_Exercise_6.ipynb` such that it tracks the training into wandb and view the results in the web interface.

----

### Exercise 5 - Voila

* Go back to the notebook which is running in virtualenv.
* Create a new python3 notebook called `webapp.ipynb`.
* Copy trained models `trained_cnn.pt`, `trained_model.pt` from Monday to the project root.
* Build an interactive webapp using [ipywidgets](https://ipywidgets.readthedocs.io/en/latest/) which allows user to specify a pretrained model to be used and image url to be specified. Then loads the selected pretrained model, computes and displays the prediction to the user.
* Click on Voila button in the jupyter notebook menu to test whether everything runs as a web app.
* Update the `.dockerfile` to include web app related files (`webapp.ipynb`, `trained_model.pt`, `trained_cnn.pt`) and rebuild your Docker image if you want to have a functional Docker container with a webapp inside.
* Push your changes to git.

----

### Exercise 6 - Binder

* Make your git repo public if it is not already.
* Go to [mybinder.org](https://mybinder.org) and fill in the form:
  * Repository URL: `https://github.com/<your_username>/oeawss21`
  * Git ref: `master` (or `main` depending on your repo)
  * Path to a **URL** (not a file): `/voila/render/webapp.ipynb`
* Copy the binder markup badge into your `readme.md`.
* Wait for app to run in Binder. It will take quite some time, but Binder is a free service, so...
* Check how to use Docker with binder if needed [here](https://mybinder.readthedocs.io/en/latest/tutorials/dockerfile.html).
* When the app runs, have fun... You can try this [image](https://m.media-amazon.com/images/I/81U64AnQvkL._AC_UL1500_.jpg) for example.

Here is an example badge to run this repo on Binder: [![Binder](https://mybinder.org/badge_logo.svg)](https://mybinder.org/v2/gh/paloha/oeawss21/master?urlpath=%2Fvoila%2Frender%2Fwebapp.ipynb)

----

### Exercise 7 - Heroku [optional]

* Create a free account on Heroku. It might still ask you to fill in your credit card though.
* Add `Procfile` with `web: voila webapp.ipynb --no-browser --port $PORT`.
* Add `runtime.txt` into project folder with `python-3.8.10`.
* Push changes to the repo.
* Install Heroku cli by following [the official guide](https://devcenter.heroku.com/articles/heroku-cli#download-and-install).
* Deploy your app to heroku using git:
  * Run `heroku update` to make sure Heroku cli is up to date
  * Run `heroku create` to create a new Heroku app
  * Run `git push heroku master` to deploy.
  * Optionally set `heroku ps:scale web=1`.
  * Openy your app with `heroku open`.
* Or follow [the deployment guide](https://voila.readthedocs.io/en/stable/deploy.html#deployment-on-heroku) directly from Voila.

----

### Some useful links
* [How to use docker with GPU](https://towardsdatascience.com/4c699c78c6d1)
* [Example on how to better structure the project](https://gitlab.com/paloha/repex-template)
* [Free Heroku slug must be at most 500MB](https://help.heroku.com/KUFMEES1/my-slug-size-is-too-large-how-can-i-make-it-smaller)
