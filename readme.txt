(A) Activate 2 virtual environments - one to run each node server
Node 1:
python3 -m venv myenv
pip install flask
pip install requests
source myenv/bin/activate

Node 2:
python3 -m venv myenv2
pip install flask
pip install requests
source myenv2/bin/activate


(B) Start both nodes on different ports:
python blockchain.py 5000
python blockchain.py 5001

(C) Register both nodes with each other

1. Register Node 2 with Node 1
curl -X POST -H "Content-Type: application/json" -d '{
 "nodes": ["http://0.0.0.0:5001"]
}' "http://0.0.0.0:5000/nodes/register"

2. Register Node 1 with Node 2
curl -X POST -H "Content-Type: application/json" -d '{
 "nodes": ["http://0.0.0.0:5000"]
}' "http://0.0.0.0:5001/nodes/register"

(D) Then can start transations and mining....

2. Add a new transaction on Node 1
curl -X POST -H "Content-Type: application/json" -d '{
 "sender": "d4ee26eee15148ee92c6cd394edd974e",
 "recipient": "someone-other-address",
 "amount": 5
}' "http://0.0.0.0:5000/transactions/new"

3. Look at pending transactions on Node 1 

curl -X GET "http://0.0.0.0:5000/transactions/uncommitted"

4. Look at blocks on Node 1 

curl -X GET "http://0.0.0.0:5000/chain"

5. Mine a new block on Node 1

curl -X GET "http://0.0.0.0:5000/mine"

6. Get new blockchain Node 1

curl -X GET "http://0.0.0.0:5000/chain"

7. Run the resolve on Node 2 (which should replace its blockchain with Node 1 as Nodes 1 is longer)

curl -X GET "http://0.0.0.0:5001/nodes/resolve"


