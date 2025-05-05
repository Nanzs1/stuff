# Let's generate the Python script based on the code provided by the user

code = """
import requests
from flask import Flask, request, jsonify

app = Flask(__name__)

MORALIS_API_KEY = "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJub25jZSI6IjM2ZGYzNWRlLTlhNjEtNDc1Mi04Njk4LTM3YTEzMDNiNTNlYSIsIm9yZ0lkIjoiNDQ1NDE0IiwidXNlcklkIjoiNDU4Mjc3IiwidHlwZUlkIjoiYWQxZTEyNmMtNWM3Yi00ZDZlLTgxNTItOGE5MDI4MTRmYWM4IiwidHlwZSI6IlBST0pFQ1QiLCJpYXQiOjE3NDY0NTg4MDMsImV4cCI6NDkwMjIxODgwM30.Zj6VQdg7dv8Fbs41bQKMU_pVB_wdX64Tw727wK-B2Kc"
NFT_CONTRACT = "0x5d541b55763a9277f61a739f40b6021a16c2d3d8"
ARBITRUM_CHAIN = "arbitrum"

def wallet_has_epic_nft(wallet_address):
    url = f"https://deep-index.moralis.io/api/v2.2/{wallet_address}/nft/{NFT_CONTRACT}"
    headers = {
        "accept": "application/json",
        "X-API-Key": MORALIS_API_KEY
    }
    params = {
        "chain": ARBITRUM_CHAIN,
        "format": "decimal",
        "normalizeMetadata": "true"
    }

    response = requests.get(url, headers=headers, params=params)

    if response.status_code != 200:
        return None

    data = response.json()
    nfts = data.get("result", [])

    for nft in nfts:
        metadata = nft.get("normalized_metadata", {})
        attributes = metadata.get("attributes", [])
        for attr in attributes:
            if attr.get("trait_type") == "Tier" and attr.get("value") == "Epic":
                return True

    return False

@app.route('/vulcan-webhook', methods=['POST'])
def vulcan_webhook():
    data = request.get_json()

    if not data:
        return jsonify({"success": False}), 400

    wallets = []

    if "wallet" in data:
        wallets = [data["wallet"]]
    elif "wallets" in data:
        wallets = data["wallets"]

    for wallet in wallets:
        result = wallet_has_epic_nft(wallet)
        if result is None:
            return jsonify({"error": "Wallet not found"}), 404
        if result is True:
            return jsonify({"success": True}), 200

    return jsonify({"success": False}), 200

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=10000)
"""

# Save the file to the system
file_path = '/mnt/data/vulcan_webhook.py'
with open(file_path, 'w') as f:
    f.write(code)

file_path
