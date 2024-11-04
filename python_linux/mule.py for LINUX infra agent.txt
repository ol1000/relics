import requests
import json

# Define MuleSoft API details
token_url = "https://anypoint.mulesoft.com/accounts/api/v2/oauth2/token"
data_url = "https://anypoint.mulesoft.com/monitoring/api/organizations/18d726bc-bc7f-42e3-9c91-23ffaf9bdd34/environments/70839385-2b5b-4cbb-99c9-bdcf6ace3f39/reports/performance/from/now-7d/to/now"

# OAuth details
client_id = "45f31a91eb6644d1a25758131285d8cf"
client_secret = "8802C7033Bf64e98a484a7bF704847b3"

def get_access_token():
    # Request token
    response = requests.post(
        token_url,
        headers={"Content-Type": "application/x-www-form-urlencoded"},
        data={
            "client_id": client_id,
            "client_secret": client_secret,
            "grant_type": "client_credentials"
        }
    )
    response.raise_for_status()  # Check for HTTP errors
    return response.json().get("access_token")

def get_performance_data(access_token):
    # Make the performance data request
    headers = {"Authorization": f"Bearer {access_token}"}
    response = requests.get(data_url, headers=headers)
    response.raise_for_status()  # Check for HTTP errors
    return response.json()

try:
    # Retrieve token
    token = get_access_token()
    
    # Retrieve performance data
    data = get_performance_data(token)

    # Print valid JSON output
    output = {
        "access_token": token,
        "performance_data": data
    }
    print(json.dumps(output, indent=2))  # Print formatted JSON

except requests.exceptions.RequestException as e:
    print("Error:", e)
