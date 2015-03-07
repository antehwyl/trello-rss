# trello-rss

Trello is a great app, but unfortunately doesn't currently have any RSS feed features. If Fog Creek decides to add it in the future, all the better. However, based on [this] (https://trello.com/card/rss-feeds/4d5ea62fd76aa1136000000c/898), it doesn't seem like a high priority right now.

I decided to throw together some Python to create RSS feeds for your Trello boards. This is useful for getting a broad overview of actions on your Trello boards delivered to your favorite RSS reader.

You're welcome to extend this into your own app, or use [my web app to generate Trello RSS feeds](http://trellorss.appspot.com).

## Dependencies

- [sarumont's py-trello] (https://github.com/sarumont/py-trello/) API wrapper. 
- [PyRSS2Gen] (https://pypi.python.org/pypi/PyRSS2Gen) is used to create the actual RSS object.
- Python 2.x. No Python 3 support right now.
- A [Trello API Key] (https://trello.com/1/appKey/generate) and [authentication token] (https://trello.com/docs/gettingstarted/index.html#getting-a-token-from-a-user). A token is not necessary if you are viewing public boards.

## API Key and Auth Token

You need an API Key to use the Trello API, and an auth token to let this app read your Trello boards. Get a key [here] (https://trello.com/1/appKey/generate) and instructions on getting a token [here] (https://trello.com/docs/gettingstarted/index.html#getting-a-token-from-a-user). (Tokens are not necessary for public boards.)

## Installation instructions

Until I get some packaging setup, the easiest thing for now is to just place this project, py-trello, and PyRSS2Gen in the same directory.

## Usage

Usage is pretty basic. You create a TrelloRSS object, with these parameters:

    TrelloRSS(apikey, [, token][, channel_title][, rss_channel_link][, rss_description])

If you don't pass in **token** we assume you're looking at public boards only. The channel info is used to create your channel info for the RSS feed. It defaults to "My Trello RSS Feed" for the title, "http://trello.com" for the link, and "Trello RSS Feed" for the description.

If you did pass in a token, you can retrieve info on all of your boards using this:

    from trellorss import TrelloRSS
    my_rss = TrelloRSS(apikey, apiprivatekey, token)
    my_rss.get_all(20) # Gets the 20 most recent actions
    print my_rss.rss # This will print the xml generated by PyRSS2Gen. Pass this into your favorite feed generator.

You can also specify what types of actions to make the feed on. (See below for supported actions.) 

    my_rss = TrelloRSS(apikey, apiprivatekey, token)
    my_rss.get_only(['comments','lists'])
    print my_rss.rss

These are the available actions:

* 'comments'
* 'board'
* 'lists'
* 'cards'
* 'createChecklist'
* 'updateCheck'
* 'moveCard'

Passing in anything else will result in an exception.

You can also create a feed on public boards. You'll need to get the board id out of the url (should look like this: nC8QJJoZ). Let's use the [Trello Development board](https://trello.com/b/nC8QJJoZ/trello-development) as an example.

    public_rss = TrelloRSS(apikey)
    public_rss.get_from('nC8QJJoZ', public_board=True, num=30)

get_from takes these parameters:

    TrelloRSS.get_from(board_id, [, public_board][, items][, number])

public_board defaults to False, items default to all, and number defaults to 15.

