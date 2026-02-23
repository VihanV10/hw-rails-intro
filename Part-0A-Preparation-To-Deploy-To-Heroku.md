
## Initial Heroku Deployment

Log in to your Heroku account by typing the command: `heroku login -i` in the terminal. Provide your nyu.edu 
(or approved alternative) email but when it asks for a password, instead you must find your **API Key** from the 
bottom of the [Account Settings page on Heroku](https://dashboard.heroku.com/account). Copy and paste this value 
in for the password.

For CHIPS 5.3, you will need to complete the Heroku deployment to your own individual Heroku app. 

Use the Heroku CLI (as you have in prior CHIPS) to create a new Heroku app for this CHIP. Once created, add Heroku to 
your local git repo as a remote named `heroku`, as we have done in the past, so you can deploy to the app.

```bash
heroku create hw-rails-intro
heroku git:remote -a hw-rails-intro
```

A "stack" is a term that describes the operating system and default software that you application is running on. 
Heroku has a [large set of stacks](https://devcenter.heroku.com/articles/stack) you can select from. In this case, 
`heroku-24` (the default stack) includes the correct versions of Rails.

If you haven't yet made a commit to your new branch, do that now (you probably have a change in the `db` folder):

```bash
git status # make sure you're on your new branch
git add [..] # stage the updated files
git commit -m [..] # write a message
```

Now, push your current branch to the Heroku remote's `main` branch:

```bash
git push heroku <YOUR_BRANCH>:main
```

(You may see the following warning the first time - it's fine. Answer "yes", and in the future you shouldn't see it anymore:)

    The authenticity of host 'heroku.com (50.19.85.132)' can't be established.
    RSA key fingerprint is 8b:48:5e:67:0e:c9:16:47:32:f2:87:0c:1f:c8:60:ad.
    Are you sure you want to continue connecting (yes/no)?
    Please type 'yes' or 'no':

Is the app running on Heroku? If you navigate to the heroku URL that is printed above the blue text at the end of the results from `git push heroku master` you'll get a "We're sorry, but something went wrong." error in the browser.

As with the previous assignment "Hello Rails", `heroku logs` tell us that the `movies` table doesn't exist. So, as before, run the initial migration and import the seed data:

```sh
heroku run bundle exec rails db:setup
# or, if that doesn't work:
heroku run bundle exec rails db:migrate
heroku run bundle exec rails db:seed
```

Since you're starting from a fresh Heroku app, the deployment should have detected your Postgres dependency and added the Postgres addon for you. If not, though, you'll get an error from the rake commands, and you should attach the addon manually:

```sh
heroku addons -a <HEROKU_APP_NAME> # make sure the postgres addon doesn't already exist
heroku addons:create heroku-postgresql -a <HEROKU_APP_NAME> # if necessary
```

Now you should be able to navigate to your app's URL.

**Note:** don't proceed past this point until you are able to complete the above successfully, or you won't be able to receive a grade for this assignment!
