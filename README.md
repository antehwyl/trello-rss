# trello-rss

Trello is a great app, but unfortunately doesn't currently have any RSS feed features. If they decided to add it in the future, all the better. However, based on [this] (https://trello.com/card/rss-feeds/4d5ea62fd76aa1136000000c/898), it doesn't seem like a high priority (if they plan on it at all). That card has 114 votes, but was archived and hasn't been updated in months.

I decided to throw together some Python to create an RSS feed for your Trello account. I personally find this useful, as I have a lot of Trello boards, but I don't always have Trello in an open browser window. This is particularly useful if you've got multiple group boards you are participating in, and would like to get a quick glance at what's happened over the past day or so, over all your boards.

You're welcome to extend this into your own app, or use [my web app to generate Trello RSS feeds](http://trellorss.appspot.com).

## Dependencies

- [sarumont's py-trello] (https://github.com/sarumont/py-trello/) API wrapper. 
- [PyRSS2Gen] (https://pypi.python.org/pypi/PyRSS2Gen) is used to create the actual RSS object.
- Python 2.x. No Python 3 support right now.
- A [Trello API Key] (https://trello.com/1/appKey/generate) and [authentication token] (https://trello.com/docs/gettingstarted/index.html#getting-a-token-from-a-user). A token is not necessary if you are viewing public boards.

## API Key and Auth Token

You need an API Key to use the Trello API, and an auth token to let this app read your Trello boards. Get a key [here] (https://trello.com/1/appKey/generate) and instructions on getting a token [here] (https://trello.com/docs/gettingstarted/index.html#getting-a-token-from-a-user). (No token necessary for public boards.)

## Installation instructions

Until I get some packaging setup, the easiest thing for now is to just place this project, py-trello, and PyRSS2Gen in the same directory.

## Usage

Usage is pretty basic. You create a TrelloRSS object, with these parameters:

    TrelloRSS(apikey, [, token][, channel_title][, rss_channel_link][, rss_description])

If you don't pass in **token** we assume you're looking at public boards only. The channel info is used to create your channel info for the RSS feed. It defaults to "My Trello RSS Feed" for the title, "http://trello.com" for the link, and "Trello RSS Feed" for the description.

If you did pass in a token, you can retrieve info on all of your boards using this:

    from trellorss import TrelloRSS
    my_rss = TrelloRSS(apikey, token)
    my_rss.get_all(20) # Gets the 20 most recent actions
    print my_rss.rss # This will print the xml generated by PyRSS2Gen. Pass this into your favorite feed generator.

You can also specify what types of actions to make the feed on. Currently, it only supports comments, new cards, new boards, and new lists. (The rest will be added soon.) Pass in what you want as a list like so:

    my_rss = TrelloRSS(apikey, token)
    my_rss.get_only(['comments','lists'])
    print my_rss.rss

These are the available params for types of actions currently:

* 'comments'
* 'board'
* 'lists'
* 'cards'
* 'createChecklist'
* 'updateCheck'

Passing in anything else will result in an exception.

You can also create a feed on public boards. You'll need to get the board id out of the url (should look like this: nC8QJJoZ). Let's use the [Trello Development board](https://trello.com/b/nC8QJJoZ/trello-development) as an example.

    public_rss = TrelloRSS(apikey)
    public_rss.get_from('nC8QJJoZ', public_board=True, num=30)

get_from takes these parameters:

    TrelloRSS.get_from(board_id, [, public_board][, items][, number]

public_board defaults to False, items default to all, and number defaults to 15.

## Security

This should be obvious, but figured I should at least mention it. If you've got important confidential info on your Trello board, this module currently will just read it all (if you give it the appropriate token, that is). If you then throw that feed onto some website, all that info is now publicly accessible (in theory). 

## TODO

* All the functionaly I wanted from this is now present. If there's something you feel it needs, please feel free to open an issue or send a pull request.
