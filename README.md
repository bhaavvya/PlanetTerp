## PlanetTerp

<https://planetterp.com/>

PlanetTerp is a website designed to help students at the University of Maryland — College Park (UMD) make informed decisions. We have a professor review system, grades for each course and professor, and tools to provide information related to UMD.

This is the second version of PlanetTerp. The first version was written in [webpy](https://github.com/webpy/webpy), and was closed source. It was rewritten in [django](https://github.com/django/django) at the beginning of 2022, which is the version you see here.

All contributions are welcome, whether that be opening an issue with a bug or feature request, or opening a pull requets. See below for how to set up PlanetTerp locally.

### Setup

PlanetTerp is developed against python 3.9. It's likely that 3.7+ will work as well, but we only officially support 3.9+.

You'll also need to set up [MySQL](https://www.mysql.com/) if you haven't already. PlanetTerp is developed against MySQL 8.0, but any reasonably recent version should work.

Once you have MySQL, you'll need to create a database for PlanetTerp. This can be named anything, but you'll probably want to call it something like `planetterp`.

Now we can start setting up PlanetTerp:

```bash
git clone https://github.com/planetterp/planetterp
cd planetterp
pip install -r requirements.txt
cp planetterp/config.py.example planetterp/config.py
```

At this point, you should open `planetterp/config.py` (not `planetterp/config.py.example`!). It should look something like this:

```python
DB_ENGINE = 'django.db.backends.mysql'
DB_NAME = "planetterp"
DB_HOST = "127.0.0.1"
USER = 'root'
PASSWORD = ''
SECRET_KEY = 'django-insecure-zt7yxn++)bh)j#wzb)ofgd0^scu9rwr85%=3l%6g1zt(cx!t)_'

WEBHOOK_URL_HELP = None
WEBHOOK_URL_UPDATE = None

EMAIL_HOST_USER = None
EMAIL_HOST_PASSWORD = None
```

If you named your database something other than `planetterp`, you'll need to edit `DB_NAME`. If you would like to use a different user than `root` to acces mysql, you'll need to edit `USER`, and if your password for your user is not the empty string, you'll need to edit `PASSWORD`.

The other fields can be left along for now. We'll discuss the `WEBHOOK_*` and `EMAIL_*` settings in a bit, but they aren't necessary for an initial setup and can be left `None`.

Once you're satisfied with your config, continue with your setup:

```bash
python manage.py migrate
# we provide some initial data (reviews, courses, professors,
# grades, users, etc) for developers. If you prefer to start from
# a completely blank database, skip this step.
python manage.py loaddata initial
```

Now simply run the following to start the server:

```bash
python manage.py runserver
```

#### Optional Setup

If you'd like to test features relating to the webhooks or emails, you'll need to fill in the relevant settings in `planetterp/config.py`.

For webhooks, you can create a webhook url by going to a discord server (which you have the "Manage Webhooks" permission in), right clicking a channel, and selecting "integrations". Then click "Create Webhook" and then "Copy Webhook URL". That value is what you'll set the webhook url setting (either `WEBHOOK_URL_HELP` or `WEBHOOK_URL_UPDATE`) to.

For emails, you'll need to specify your email and password in `EMAIL_HOST_USER` and `EMAIL_HOST_PASSWORD` respectively. Make sure to include the `@gmail` portion in `EMAIL_HOST_USER`. Feel free to create a burner gmail account for this purpose if you don't feel comfortable using your personal email address.

It's very likely that you'll also need to turn on "less secure app access" for your gmail account for email sending to work. This setting can be found under <https://myaccount.google.com/security> -> "Less secure app access".

### Contributing

All contributions are welcome!

If you're submitting a PR, see [STYLE.md](./STYLE.md) for a style guide, but you'll be safe if you follow existing conventions in the codebase.
