---
title: "Jinaga"
---

So far we have cached content and application code so that the user has access while offline.
But what about their data?
We want to store the user's data while they are offline so that they can still see it when they open up the app.
And we also want to persist any changes that the user makes so that we can send it up when they reconnect.
Jinaga does all of this splendidly.

To get started, clone and detach the Jinaga starter.
We'll use the one based on JavaScript and React.

```bash
git clone https://github.com/jinaga/starter-javascript-react.git jinagapwa
cd jinagapwa
rm -rf .git
```

Then build and run the application.

```bash
npm run build
npm run dev
```

The application starts at http://localhost:8080/.
You should see a link asking you to log in on Twitter.
For this part to work, you will need a Twitter API key.

### Twitter Login

You can get an API key from your [Twitter Apps Dashboard](https://developer.twitter.com/en/apps).
Sign up for a developer account and create a new app.
Be sure to enable Sign In with Twitter.
Add a callback URL for `http://localhost:8080/auth/twitter/callback`.
If you plan to deploy to the cloud, you can also add a Netlify or Azurewebsites URL as well, using the same path.

Once you create your application, you will get a Consumer API Key and Consumer API Secret Key.
These will go into environment variables on your machine.
On a Mac, add these lines to '~/.bash_profile`:

```bash
export TWITTER_CONSUMER_KEY=xxxxxxxxxxxxxxxxxxxxxxxxx
export TWITTER_CONSUMER_SECRET=yyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyy
```

On Windows, use System Settings to change environment variables.

Stop the application, close the terminal window, and restart it again for these changes to take effect.
You will be able to click the link and log in with any Twitter account (not just the developer account).
But, you will run into the next issue.

### Create the Database

Now the application will try to save something to a database.
The database it uses is Postgres.
The default Postgres connection string is in `src/server/jinaga-config.ts`, or you can change it with the `JINAGA_POSTGRESQL` environment variable.
We are just going to change the default value in the source code.
Change the name of the database to `jinagapwa`, so that the whole connection string is `postgresql://dev:devpw@localhost:5432/jinagapwa`.
This will restart the application automatically.

Now you need to create that database.
Make sure you have [PostgreSQL](https://www.postgresql.org/download/) installed and running.
On MacOS, you can use homebrew:

```bash
brew install postgresql
```

On Windows, download and run the installer.

Then you can create the database and the application user:

```bash
echo "CREATE DATABASE jinagapwa;" | psql -h localhost -U postgres postgres
echo "CREATE USER dev WITH LOGIN ENCRYPTED PASSWORD 'devpw' VALID UNTIL 'infinity';" | psql -h localhost -U postgres jinagapw
```

Finally, create the database tables.

```bash
psql -h localhost -f node_modules/jinaga/setup.sql -U postgres jinagapwa
```

Now when you log in, you should see some action.
