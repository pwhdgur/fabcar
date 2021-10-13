< Hyperledger Fabric v2.2 Fabcar >
reference site : https://kctheservant.medium.com/rework-an-implementation-of-api-server-for-hyperledger-fabric-network-fabric-v2-2-a747884ce3dc

1. 변경 사항 From v 1.4 to v 2.2
●  One major difference is the way to create wallet objects. In v1.4 it was handled by FileSystemWallet class. In v2.2 it is the class Wallets handling wallet in client applications. 
●  You can notice the change in the code (line 20, 65, 109 and 152).

2. Demonstration Step

Step 1: Bring up Test Network and deploy fabcar chaincode

cd fabric-samples/fabcar/
./startFabric.sh

Step 2: Generate user appUser for API server

cd javascript/
npm install
node enrollAdmin.js
node registerUser.js

Step 3: Install NodeJS environment

sudo apt-get update
sudo apt install curl
curl -sL https://deb.nodesource.com/setup_10.x | sudo bash -
sudo apt install -y nodejs
sudo apt-get install build-essential
node -v
npm -v

Step 4: Copy files from Fabric Host

mkdir apiserver
cd apiserver

● wallet (identity): fabcar/javascript/wallet/appUser.id
● node package file: fabcar/javascript/package.json
● connection profile: test-network/organizations/peerOrganizations/org1.example.com/connection-org1.json


Step 5: Copy apiserver.js to this directory. or Create apiserver.js File


Step 6: Modify connection profile

● change it from localhost to IP of Fabric Host

sed -i 's/localhost/34.203.38.3/g' connection-org1.json

Step 7: Update /etc/hosts to reach Fabric Host

● 127.0.0.0 localhost
● host IP orderer.example.com
● host IP peer0.org1.example.com
● host IP peer0.org2.example.com

Step 8: Install packages on API Server Host

npm install
npm install express body-parser --save

Step 9: Run API Server

node apiserver.js

3. Testing: on my localhost
- localhost or Host IP
● Query All Cars 
curl http://localhost:8080/api/queryallcars

● Add a new car
curl -d '{"carid":"CAR12","make":"Honda","model":"Accord","colour":"black","owner":"Tom"}' -H "Content-Type: application/json" -X POST http://localhost:8080/api/addcar

● Query the new car
curl http://localhost:8080/api/query/CAR12

● Change owner for a car
curl http://localhost:8080/api/query/CAR4
curl -d '{"owner":"Daniel"}' -H "Content-Type: application/json" -X PUT http://localhost:8080/api/changeowner/CAR4

● Query the changed car again
curl http://localhost:8080/api/query/CAR4

4. CORS Error 대응 코드 apiserver.js 추가

// CORS Origin
app.use(function (req, res, next) {
    res.setHeader('Access-Control-Allow-Origin', '*');
    res.setHeader('Access-Control-Allow-Methods', 'GET, POST, PUT, DELETE');
    res.setHeader('Access-Control-Allow-Headers', 'Origin, X-Requested-With, Content-Type, Accept, Authorization');
    res.setHeader('Access-Control-Allow-Credentials', true);
    next();
  });
  
app.use(express.json());
