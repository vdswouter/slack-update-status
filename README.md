# Slack Wifi Status

Set your Slack status to either :house_with_garden: or :coffee: (or some custom emoji) based on your location.

You list a set of locations, if you're at that place, then your status is set to

> :pick an emoji: My custom status

If you are at a location not on the list, then your status is set the `else`-default

> :coffee: At a coffee shop


## Setup

Clone the repo:

```
git clone git@bitbucket.org:mantebridts/slackupdatestatus.git
```
Install the required gems:

```
cd slack-wifi-status
bundle install
brew cask install corelocationcli
```

Create a file in `config/slack.yml` that looks like this.
You can use `config/slack.yml.example` as a template.



```yaml
oauth_key: "xoxp-this-is-a-totally-real-oauth-key"
kontich:
  address:
    - Kontich // This string is used for matchmaking against your current location, pick anything, such as a straat and number, or just the city or country
  slack:
    message: "@ HQ"
    emoji: ":house:"
else:
  slack:
    message: "Probably in a meeting ¯\_(ツ)_/¯"
    emoji: ":spiral_calendar_pad:"
```

Another example:

![picture](config.png)


Instructions for getting an OAuth-key are below.


You can test your setup by running

```
./bin/update-slack-status
```

It should update your status on Slack.

## Automatically updating your status

Setup a cron job that runs every five minutes and runs `bin/update-slack-status`, use this command to use nano to edit the crontab-file:

```export VISUAL=nano; crontab -e```

and put this line in the file

```
*/5 * * * * cd /Users/yourname/path/to/code/slack-wifi-status && ./bin/update-slack-status > /dev/null 2> /dev/null
```


## Detailed OAuth instructions

To complete the setup, you'll need an OAUTH-key for Slack and a list of locations.

Go to https://api.slack.com/apps

Create a new app for the organization you're going to be changing your status on.

You need to set the permissions needed. You need `users.profile:read` and `users.profile:write`.

Slack will then give you an OAuth access token. Copy it and stick it in `config/slack.yml`.