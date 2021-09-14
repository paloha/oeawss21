# OEAW SummerSchool on AI 2021
## Tuesday exercise session

### Exercise 1 - Jupyter in Virtualenv

* Make a volume folder
* Make a project folder inside the volume folder
* Download the solution6.ipynb from Monday
* Create requirements.txt
* Install & create a virtualenv
* Install dependencies from requirements.txt
* Run the jupyter notebook server
* Run the cells 1-4 until you see the images
* Create a readme.md file with just a name of the project

### Exercise 2 - Git
#### Make sure you can authenticate with git using ssh key

* Go to github.com > settings > SSH and GPG keys
* Copy your public ssh key from cat ~/.ssh/id_rsa.pub
* Add it to the github keys and verify with ssh -T git@github.com
* In case of different OS or problems consult:
https://docs.github.com/en/github/authenticating-to-github/connecting-to-github-with-ssh/testing-your-ssh-connection

#### Create a new repository and initialize & push your project to origin

* Go to github.com and create a new private repository `oeawss21`
* Do not check the automatic creation of readme, license or other files
* Go to your project folder
* Create .gitignore file with .venv, .ipynb_checkpoints in it
* git init
* git remote add origin git@github.com:<your_username>/oeawss21.git
* git add .
* git commit -m "Initial commit"
* git push origin master

#### More in-depth but still absolutely simple guide:

* https://rogerdudler.github.io/git-guide/

### Exercise 3 - Docker

#### Install Docker

* sudo apt install docker.io
* Check if it is installed correctly (sudo docker run hello-world)

#### Pull Ubuntu minimal docker image

* sudo docker pull `intelliseqngs/ubuntu-minimal-20.04:3.0.5`
* Add a Dockerfile into your project based on `intelliseqngs/ubuntu-minimal-20.04:3.0.5`
* Reference on writing the docker file https://docs.docker.com/engine/reference/builder/
```
  FROM intelliseqngs/ubuntu-minimal-20.04:3.0.5
  WORKDIR /home/volume/project
  COPY requirements.txt Solution_Exercise_6.ipynb webapp.ipynb ./
  RUN pip3 install -r requirements.txt
  CMD ["/bin/bash"]
```
#### Build the image

* `sudo docker build -t oeawss21:latest -f Dockerfile .`
* In case your Docker errors on "killed" Adjust Docker Preferences Resources RAM - make it bigger, i.e. 6GB
* `sudo docker run --rm -v /path/volume/data:/home/volume/data -p 8888:8888 oeawss21:latest jupyter notebook --allow-root --ip 0.0.0.0`
  `-p` forwards port 8888 of the container to 8888 on the host
  `-v` mounts host folder to containers folder (paths must be absolute)
  `--allow-root` since all in the container runs as root
  `--ip 0.0.0.0` expose the jupyter server so host can see it

* visit `localhost:8888` in your browser and copy the token, the jupyter notebook should run


### Exercise 4 - Weights and Biases

* Log in to your account and try the Example (wandb.me/intro) and run it until the "Run experiment" cell finishes
* Check the results in the wandb.ai account
* Check also these examples https://github.com/wandb/examples

### Exercise 5 - Voila

* Go back to the notebook which is running in virtualenv
* Create a new python3 notebook called `webapp.ipynb`
* Copy trained models from Monday to the project folder
* Build an interactive webapp using ipywidgets which accepts an image url loads a pretrained model and displays the prediction
* Click on Voila to test whether everything runs as web app
* Update the Dockerfile to include model files (trained_model.pt trained_cnn.pt)

### Exercise 6 - Binder

* rename the Dockerfile to .dockerfile, git push and try again
* push to the repo
* make your git repo public
* go to mybinder.org and fill in the form
  Repository URL: `https://github.com/<your_username>/oeawss21`
  Git ref: `master`
  Path to a URL: `voila/render/webapp.ipynb`
* copy the binder markup badge into your readme.md
* wait for app to run in binder
* Check how to use docker with binder if needed
  https://mybinder.readthedocs.io/en/latest/tutorials/dockerfile.html
* When the app runs, have fun...
  Try this image for example: https://m.media-amazon.com/images/I/81U64AnQvkL._AC_UL1500_.jpg

[![Binder](https://mybinder.org/badge_logo.svg)](https://mybinder.org/v2/gh/paloha/oeawss21/master?urlpath=%2Fvoila%2Frender%2Fwebapp.ipynb)

### Exercise 7 - Heroku [optional]

* add `Procfile` with `web: voila webapp.ipynb --no-browser --port $PORT`
* add `runtime.txt` into project folder with `python-3.8.10`
* push to the repo
* install Heroku cli https://devcenter.heroku.com/articles/heroku-cli#download-and-install
* deploy to heroku with git
  `heroku update # to make sure heroku cli is up to date`
  `heroku create`
  `git push heroku master`
  `heroku ps:scale web=1`
  `heroku open`
* or follow the steps here https://voila.readthedocs.io/en/stable/deploy.html#deployment-on-heroku


### Notes
* How to use docker with GPU
https://towardsdatascience.com/4c699c78c6d1

* How to better structure the project
https://gitlab.com/paloha/repex-template

* Free Heroku slug must be at most 500MB
https://help.heroku.com/KUFMEES1/my-slug-size-is-too-large-how-can-i-make-it-smaller
