# Django-Boilerplate
Helps to quickly and easily setup a production ready Django project  

# Local
The local dimension always come first. It is the settings and setup that a developer will use on their local machine.
All the defaults and configurations must be done to attend the local development environment first.
The reason why I like to do it that way is that the project must be as simple as possible for a new hire to clone the repository, run the project and start coding.
The production environment usually will be configured and maintained by experienced developers and by those who are more familiar with the code base itself. And because the deployment should be automated, there is no reason for people being re-creating the production server over and over again. So it is perfectly fine for the production setup require a few extra steps and configuration.

# Tests
The tests environment will be also available locally, so developers can test the code and run the static checks.
But the idea of the tests environment is to expose it to a CI environment like Travis CI, Circle CI, AWS Code Pipeline, etc.
It is a simple setup that you can install the project and run all the unit tests.

# Production
The production dimension is the real deal. This is the environment that goes live without the testing and debugging utilities.
I also use this “mode” or dimension to run the staging server.
A staging server is where you roll out new features and bug fixes before applying to the production server.
The idea here is that your staging server should run in production mode, and the only difference is going to be your static/media server and database server. And this can be achieved just by changing the configuration to tell what is the database connection string for example.
But the main thing is that you should not have any conditional in your code that checks if it is the production or staging server. The project should run exactly in the same way as in production.

# Requirements
Inside the project root (2) I like to create a directory called requirements with all the .txt files, breaking down the project dependencies like this:

`base.tx`t: Main dependencies, strictly necessary to make the project run. Common to all environments
`tests.txt`: Inherits from base.txt + test utilities
`local.txt`: Inherits from tests.txt + development utilities
`production.txt`: Inherits from base.txt + production only dependencies
Note that I do not have a `staging.txt` requirements file, that’s because the staging environment is going to use the production.txt requirements so we have an exact copy of the production environment.


# Note that I removed the `settings.py` that used to live inside the Boilerplate directory.
The majority of the code will live inside the `base.py` settings module.
Everything that we can set only once in the `base.py` and change its value using python-decouple we should keep in the `base.py` and never repeat/override in the other settings modules.

After the removal of the main `settings.py` a nice touch is to modify the `manage.py` file to set the `local.py` as the default settings module so we can still run commands like `python manage.py runserver` without any further parameters: