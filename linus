from flask import Flask, jsonify
import requests
from bs4 import BeautifulSoup

app = Flask(__name__)

@app.route('/linus')
def hae_linus_ruokalista():
    url = "https://www.unica.fi/ravintolat/kupittaan-kampus/linus/"
    response = requests.get(url)
    soup = BeautifulSoup(response.text, 'html.parser')

    ruokalista = {}
    viikonpaivat = ["maanantai", "tiistai", "keskiviikko", "torstai", "perjantai"]

    for paiva in viikonpaivat:
        otsikko = soup.find("h3", string=lambda s: s and paiva in s.lower())
        if otsikko:
            ruuat = otsikko.find_next("ul")
            if ruuat:
                ruokalista[paiva] = [li.get_text(strip=True) for li in ruuat.find_all("li")]
            else:
                ruokalista[paiva] = ["Ei ruokalistaa saatavilla"]
        else:
            ruokalista[paiva] = ["Ei löytynyt"]

    return jsonify(ruokalista)
