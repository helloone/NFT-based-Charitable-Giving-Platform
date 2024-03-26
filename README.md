# NFT-based-Charitable-Giving-Platform
创建一个基于NFT的慈善捐赠平台，将独特的数字资产与慈善事业联系起来，鼓励创新的筹款方式。
from flask import Flask, request, jsonify
from uuid import uuid4

# Simulating a database for demo purposes
nfts = []
charities = []

app = Flask(__name__)

# Define a Charity
class Charity:
    def __init__(self, name, description):
        self.id = str(uuid4())
        self.name = name
        self.description = description

# Define an NFT
class NFT:
    def __init__(self, title, image_url, charity_id, creator):
        self.id = str(uuid4())
        self.title = title
        self.image_url = image_url
        self.charity_id = charity_id
        self.creator = creator
        self.is_available = True

@app.route('/create_charity', methods=['POST'])
def create_charity():
    data = request.json
    charity = Charity(data['name'], data['description'])
    charities.append(charity)
    return jsonify({'id': charity.id, 'name': charity.name}), 201

@app.route('/create_nft', methods=['POST'])
def create_nft():
    data = request.json
    nft = NFT(data['title'], data['image_url'], data['charity_id'], data['creator'])
    nfts.append(nft)
    return jsonify({'id': nft.id, 'title': nft.title}), 201

@app.route('/list_nfts', methods=['GET'])
def list_nfts():
    return jsonify([{'id': nft.id, 'title': nft.title, 'is_available': nft.is_available} for nft in nfts]), 200

@app.route('/purchase_nft/<nft_id>', methods=['POST'])
def purchase_nft(nft_id):
    # In a real scenario, this method would handle transactions through a blockchain network
    for nft in nfts:
        if nft.id == nft_id and nft.is_available:
            nft.is_available = False
            return jsonify({'success': True, 'message': 'NFT purchased! Proceeds will go to the linked charity.'}), 200
    return jsonify({'success': False, 'message': 'NFT not available or does not exist.'}), 404

@app.route('/list_charities', methods=['GET'])
def list_charities():
    return jsonify([{'id': charity.id, 'name': charity.name} for charity in charities]), 200

if __name__ == '__main__':
    app.run(debug=True)
