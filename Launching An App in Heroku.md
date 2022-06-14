# Launching An App in Heroku

This documentation is intended to walk through the steps required to launch an app on Heroku. The example that will be followed is with our Game 24 Solver app.

## Creating Your Project on GitHub

We must first create a project via GitHub Desktop. We CAN NOT use git commands in the anaconda prompt as there is no way for Heroku and GitHub to know where those projects exist.

Launch GitHub on your desktop or browser. You create a project with the name of your choosing and then clone it to your local hard drive. Once you have done so, redirect yourself into the folder of your GitHub project using the <code>cd FOLDERNAME</code> command until you are in the desired folder. 

Activate or make a new environment by using the following command:

```shell
conda create --name ENVNAME --file requirements.conda
```

This will allow us to create a new environment based on a requirements file that contains all the packages we need for this particular environment and what we want to achieve.

Next, activate your environment by running <code>conda activate ENVNAME</code> into your anaconda prompt.

Now, we redirect ourselves to Heroku for a moment. We initialize a project on Heroku by going to heroku.com, login, and create a new app. Name the app, and then click create.

Return to your anaconda command prompt. 

Next, run <code>heroku login</code>. Press and key to activate the login process in your browser, and once it is complete, return back to the command prompt to continue.

We can run the line <code>heroku apps</code> to see what apps are collaborative and which are your own. 

Next, run the following line:

```shell
git remote -v
```

This pulls the collaborative apps together. Next, we just need to add the heroku remote.

```shell
heroku git:remote -a HEROKUAPPNAME
OUTPUT: set git remote heroku to https://git.heroku.com/HEROKUAPPNAME.git
```

Then, run <code>git remote -v</code> again  and the outcome should be heroku apps with fetch and push, and a github directory with fetch and push as well. 

```shell
OUTPUT
heroku  https://git.heroku.com/HEROKUAPPNAME.git (fetch)
heroku  https://git.heroku.com/HEROKUAPPNAME.git (push)
origin  https://github.com/AchilleaResearch/Game24.git (fetch)
origin  https://github.com/AchilleaResearch/Game24.git (push)
```

Run the line <code>code .</code> to open all your files in Visual Studio Code (VSC).

## Creating a Secret Key for your App

A secret key is important to make in order to keep your heroku app protected in some way. 

If you have a membership with Heroku, you can run the following line:

```shell
heroku addons:create securekey --app APPNAME
```

However, a membership is needed for this and there is an easier way to go about this process. 
In your command prompt, run <code>python manage.py shell</code>. This will open a python shell within your command prompt.

Run the following lines:

```shell
from django.core.management.utils import get_random_secret_key
get_random_secret_key()
```

A secret key will appear. Copy it into Notepad++ for now to safe-keep it. Run <code>exit()</code> to exit the shell in your command prompt. 

Now, we take the secret key and input it into our state file. This can be found in the following path:

```shell
User/anaconda3/envs/ENVNAME/conda_meta/state
```

Within your state file, the secret key should be formatted as follows:
<code>{"env_vars": {"SECRET_KEY": "SECRET-KEY-COPIED-FROM-SHELL"}}</code>

Run <code>conda deactivate</code> and <code>conda activate ENVNAME</code> to restart your environment after the changes. Next, run <code>echo %SECRET_KEY%</code>to ensure the secret key has been sucessfully saved.
If this does not return the correct value, run <code>conda env config vars list</code> to do a check. This will usually show the correct values. If not, try again with saving the secret key in the state file, and then deactivating/reactivating your environment. 

Then, go to Heroku and then to the settings of your particular app. Under the "Config Vars" section, your secret key needs to be specified. Type "SECRET_KEY" in the Key box, and copy the secret key value into the Value box. 

Run <code>heroku config</code> for one final check that the secret key got fully placed in the Heroku app.

## Creating a requirements.txt file

We need to now create a requirements.txt file. This stores all the information on the libraries and packages for a particular project. 

We run the following line:

```shell
pip list --format=freeze > requirements.txt
```

For most projects, it is important to include gunicorn. We include this at the end of the requirements.txt file with <code>gunicorn==20.1.0</code>

## Settings.py file for Project
We need to include a django-heroku bridge within our app. We go to the settings.py file for your project, scroll to the end of the settings.py file and type the following:

```shell
import django_heroku
django_heroku.settings(locals())
```

This will pull your local settings and configurations from your local drive to heroku and "bridge the gap".

## Load Your Database Locally

In local mode, you can load your database if you have one, by simply running the following line:

```shell
python manage.py loaddata FILENAME.json
```

This easily transfers all intended data into your app and project for easier manipulation and viewing from your admin page; this also allows for testing before pushing to Heroku. 

If you created a database separately and you would like to place it on Heroku, you need a data.json file and an existing heroku app. 
This json file must exist within your project directory. 

```shell
type FILENAME.json | heroku run --no-tty -a APPNAME -- python manage.py loaddata --format=json -
```

## Pushing to Heroku from Git

We are now ready to push from your local Git to Heroku. We run the following lines:

```shell
git status
```

This ensures we are on the correct branch and ready to push.

```shell
git remote -v
```

This ensures that we are connecting the correct app in Git to the correct app in Heroku.

```shell
git push heroku BRANCHNAME:master/main (depending on what it is called)
```

This will push to your Heroku app, yay! If this is also a django linked app, this will also create a database url that will be contained within your config vars in heroku settings.
