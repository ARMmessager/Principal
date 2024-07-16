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
