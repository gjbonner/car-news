from flask import Flask
from flask_ask import Ask, statement, question, session
import json
import requests
import time
import unidecode

app = Flask(__name__)
ask = Ask(app, "/auto_news")


#I removed my username and password for obvious reasons
def get_headlines():
    user_pass_dict = {'user': 'username',
                      'passwd': 'password',
                      'api_type': 'json'}
    sess = requests.Session()
    sess.headers.update({'User-Agent': 'I am testing my echo'})
    sess.post('https://www.reddit.com/api/login', data = user_pass_dict)
    time.sleep(1)
    url = 'https://reddit.com/r/autonews/.json?limit=10'
    html = sess.get(url)
    data = json.loads(html.content.decode('utf-8'))
    titles = [unidecode.unidecode(listing['data']['title']) for listing in data['data']['children']]
    titles = '... '.join([i for i in titles])
    return titles



@app.route('/')
def homepage():
    return "page test"

@ask.launch
def start_skill():
    welcome_message = 'Hi Giles, want to hear car news?'
    return question(welcome_message)

@ask.intent("YesIntent")
def share_headlines():
    headlines = get_headlines()
    headline_msg = 'New things in the car world are, {}'.format(headlines)
    return statement(headline_msg)


@ask.intent("NoIntent")
def no_intent():
    bye_text = 'okay, bye'
    return statement(bye_text)
    
if __name__ == '__main__':
    app.run(debug=True)
