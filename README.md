# Principal
https://github.com/JynxC98/quantitative_finance/blob/main/option-data/fetch_data.py
https://github.com/EvanDietrich/Quant-Finance-Projects
https://github.com/goldmansachs/gs-quant
https://github.com/alisonmitchell/Stock-Prediction


The error message you are encountering indicates that there is a problem with your proxy settings, specifically that the password for the proxy has expired. Here are some steps to resolve this issue:

### Steps to Resolve Proxy Error

1. **Update Proxy Credentials**:
   - Make sure that your proxy credentials are up-to-date. Check with your network administrator to get the correct username and password.

2. **Configure Proxy Settings in VS Code**:
   - Open VS Code and go to `File` -> `Preferences` -> `Settings`.
   - Search for `proxy`.
   - Under `HTTP: Proxy`, enter the updated proxy URL, including the new username and password.

3. **Set Proxy in yfinance**:
   You can set the proxy settings in your yfinance code directly by using the `requests` library to set the proxy globally or by configuring it in each request.

   ```python
   import yfinance as yf
   import requests
   from requests.adapters import HTTPAdapter
   from requests.packages.urllib3.util.retry import Retry

   proxy = "http://your_proxy_username:your_proxy_password@proxy_address:proxy_port"
   proxies = {
       "http": proxy,
       "https": proxy,
   }

   session = requests.Session()
   retry = Retry(connect=3, backoff_factor=0.5)
   adapter = HTTPAdapter(max_retries=retry)
   session.mount('http://', adapter)
   session.mount('https://', adapter)
   session.proxies.update(proxies)

   data = yf.download("ML.PA", session=session)
   print(data)
   ```

4. **Check Firewall and Network Restrictions**:
   - Ensure that your firewall or any network restrictions are not blocking the proxy or the yfinance request.

5. **Test the Proxy**:
   - Verify that the proxy is working by using it with a different tool or script to ensure that the issue is not specific to yfinance.

6. **Update yfinance and Dependencies**:
   - Make sure you have the latest version of yfinance and its dependencies. You can update it using pip:
     ```bash
     pip install --upgrade yfinance
     ```

### Example Code to Fetch Data Using yfinance with Proxies

Here's an example code snippet that incorporates the proxy settings:

```python
import yfinance as yf
import requests

proxy = "http://your_proxy_username:your_proxy_password@proxy_address:proxy_port"
proxies = {
    "http": proxy,
    "https": proxy,
}

session = requests.Session()
session.proxies.update(proxies)

data = yf.download("ML.PA", session=session)
print(data)
```

Ensure you replace `your_proxy_username`, `your_proxy_password`, `proxy_address`, and `proxy_port` with your actual proxy details.

By following these steps, you should be able to resolve the proxy error and fetch data using yfinance in VS Code.

import pandas as pd
import re

#Charger le calendrier d'événements financiers depuis un fichier Excel
calendrier = pd.read_excel('calendrier_financier.xlsx')

#Définir les mots-clés ou expressions régulières pour les événements d'intérêt
mots_cles = ['GDP', 'CPI', 'Interest Rate', 'Employment Report']

#Fonction pour vérifier si un événement correspond aux mots-clés
def evenement_interessant(evenement, mots_cles):
    for mot in mots_cles:
        if re.search(mot, evenement, re.IGNORECASE):
            return True
    return False

#Filtrer les événements intéressants
evenements_interessants = calendrier[calendrier['Event'].apply(lambda x: evenement_interessant(x, mots_cles))]

#Sauvegarder les événements filtrés dans un nouveau fichier Excel
evenements_interessants.to_excel('evenements_interessants.xlsx', index=False)

#Afficher les événements filtrés
print(evenements_interessants)



Pour trier et extraire les événements financiers qui vous intéressent à partir d'un calendrier complet, vous pouvez utiliser un code Python qui charge le calendrier à partir d'un fichier Excel, filtre les événements en fonction de mots-clés ou de critères définis, et extrait uniquement ceux qui sont d'intérêt. Voici comment vous pouvez procéder :

1. **Charger le fichier Excel :** Utiliser `pandas` pour lire le fichier Excel contenant le calendrier des événements.
2. **Définir les critères de filtrage :** Utiliser des mots-clés ou des expressions régulières pour identifier les événements d'intérêt.
3. **Filtrer les événements :** Parcourir le calendrier et extraire les événements correspondant aux critères définis.
4. **Sauvegarder ou afficher les événements filtrés :** Exporter les résultats dans un nouveau fichier Excel ou les afficher.

Voici un exemple de code pour accomplir cela :

```python
import pandas as pd
import re

# Charger le calendrier d'événements financiers depuis un fichier Excel
calendrier = pd.read_excel('calendrier_financier.xlsx')

# Définir les mots-clés ou expressions régulières pour les événements d'intérêt
mots_cles = ['GDP', 'CPI', 'Interest Rate', 'Employment Report']

# Fonction pour vérifier si un événement correspond aux mots-clés
def evenement_interessant(evenement, mots_cles):
    for mot in mots_cles:
        if re.search(mot, evenement, re.IGNORECASE):
            return True
    return False

# Filtrer les événements intéressants
evenements_interessants = calendrier[calendrier['Event'].apply(lambda x: evenement_interessant(x, mots_cles))]

# Sauvegarder les événements filtrés dans un nouveau fichier Excel
evenements_interessants.to_excel('evenements_interessants.xlsx', index=False)

# Afficher les événements filtrés
print(evenements_interessants)
```

### Explication du Code

1. **Chargement du fichier Excel :**
   ```python
   calendrier = pd.read_excel('calendrier_financier.xlsx')
   ```
   Cette ligne charge les données du fichier `calendrier_financier.xlsx` dans un DataFrame `calendrier`.

2. **Définition des mots-clés :**
   ```python
   mots_cles = ['GDP', 'CPI', 'Interest Rate', 'Employment Report']
   ```
   Ici, nous définissons une liste de mots-clés représentant les événements qui vous intéressent.

3. **Fonction de filtrage :**
   ```python
   def evenement_interessant(evenement, mots_cles):
       for mot in mots_cles:
           if re.search(mot, evenement, re.IGNORECASE):
               return True
       return False
   ```
   Cette fonction vérifie si un événement contient l'un des mots-clés définis.

4. **Filtrage des événements :**
   ```python
   evenements_interessants = calendrier[calendrier['Event'].apply(lambda x: evenement_interessant(x, mots_cles))]
   ```
   Nous utilisons la méthode `apply` pour appliquer la fonction de filtrage à chaque événement du calendrier.

5. **Sauvegarde et affichage :**
   ```python
   evenements_interessants.to_excel('evenements_interessants.xlsx', index=False)
   print(evenements_interessants)
   ```
   Les événements filtrés sont sauvegardés dans un nouveau fichier Excel et affichés.

### Remarques

- Assurez-vous que la colonne contenant les noms des événements dans votre fichier Excel s'appelle bien `Event`. Si ce n'est pas le cas, remplacez `Event` par le nom correct de la colonne.
- Vous pouvez ajuster les mots-clés et les expressions régulières selon vos besoins pour une meilleure correspondance avec les événements d'intérêt.

Ce code devrait vous permettre de filtrer et d'extraire efficacement les événements financiers qui vous intéressent à partir de votre calendrier complet.


   from datetime import date, timedelta
import pandas as pd
import requests
from pandas import DataFrame

def request(url: str, method: str = "get", timeout: int = 0, **kwargs) -> requests.Response:
    method = method.lower()
    if method not in ["delete", "get", "head", "patch", "post", "put"]:
        raise ValueError(f"Invalid method: {method}")
    headers = kwargs.pop("headers", {})
    headers["User-Agent"] = headers.get("User-Agent", 
                                        "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 "
                                        "(KHTML, like Gecko) Chrome/91.0.4472.124 Safari/537.36")
    timeout = timeout or 10
    func = getattr(requests, method)
    return func(url, headers=headers, timeout=timeout, **kwargs)

def get_filters(date_str: str) -> str:
    return f"?filter[selected_date]={date_str}&filter[with_rating]=false&filter[currency]=USD"

class EventsFetcher:
    def __init__(self):
        pass
    
    @staticmethod
    def get_next_earnings(limit: int = 5, start_date: date = date.today()) -> DataFrame:
        base_url = "https://seekingalpha.com/api/v3/earnings_calendar/tickers"
        df_earnings = pd.DataFrame()
        
        for _ in range(0, limit):
            start_date = pd.to_datetime(start_date)
            date_str = str(start_date.strftime("%Y-%m-%d"))
            response = request(base_url + get_filters(date_str), timeout=10)
            json = response.json()
            
            try:
                data = json["data"]
                cleaned_data = [x["attributes"] for x in data]
                temp_df = pd.DataFrame.from_records(cleaned_data)
                temp_df = temp_df.drop(columns=["sector_id"])
                temp_df["Date"] = start_date  # pylint: disable=E1137
                df_earnings = pd.concat([df_earnings, temp_df], join="outer", ignore_index=True)
                start_date = start_date + timedelta(days=1)
            except KeyError:
                pass
        
        df_earnings = df_earnings.rename(
            columns={
                "slug": "Ticker",
                "name": "Name",
                "release_time": "Release Time",
                "exchange": "Exchange",
            }
        )
        
        if df_earnings.empty:
            print("No earnings found. Try adjusting the date.\n")
            return pd.DataFrame()
        
        df_earnings = df_earnings[df_earnings["Date"] <= pd.to_datetime(start_date + timedelta(days=limit))]
        return df_earnings

if __name__ == "__main__":
    events_fetcher_ = EventsFetcher()
    upcoming_events_ = events_fetcher_.get_next_earnings()
    print(upcoming_events_)
