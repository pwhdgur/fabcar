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





Sudo Complex String (hyosungitxTest)


{
    "complex": "hyosungitxTest",
    "base64": "aHlvc3VuZ2l0eFRlc3QtOEl5ZVJzUUwyU1drWTUzSzpoeW9zdW5naXR4VGVzdC1zZWNyZXQteW1rcW5ZSk5sSWVDdmcwbnZHVkczYUI2cmhTZldQcWE=",
    "secret": "$2a$10$23w9SysSOrLCotoPDS31ZuAmiNEqwdnFm62aiaZ82BelD3ac3UCXO",
    "client_secret": "hyosungitxTest-secret-ymkqnYJNlIeCvg0nvGVG3aB6rhSfWPqa",
    "client_id": "hyosungitxTest-8IyeRsQL2SWkY53K"
}


// Security ClientDetail
{
    "_id": {
        "$oid": "615fced1f36dca0a0d979971"
    },
    "clientId": "hyosungitxTest-8IyeRsQL2SWkY53K",
    "clientSecret": "$2a$10$23w9SysSOrLCotoPDS31ZuAmiNEqwdnFm62aiaZ82BelD3ac3UCXO",
    "clientName": "효성ITX TEST",
    "complexCode": "hyosungitxTest",
    "description": "효성ITX TEST",
    "scopes": ["read", "write"],
    "resourceIds": ["complex"],
    "grantTypes": ["client_credentials"],
    "redirectUris": [],
    "authorities": [],
    "accessTokenValidity": 0,   //추가 필드 
    "refreshTokenValidity": 0,
    "autoApprove": false
}


// complex
{
    "_id": {
        "$oid": "615fdce4f36dca0bbdd45db3"
    },
    "code": "hyosungitxTest",
    "region": "서울",
    "name": "효성ITX TEST",
    "nameEng": "hyosungITX TEST",
    "hostUrl": "http://hyosungtest-api.hyosungitx.com",
    "home": "http://hyosungtest.hyosungitx.net",
    "logo": "",
    "locale": "KR",
    "basicSecret": "aHlvc3VuZ2l0eFRlc3QtOEl5ZVJzUUwyU1drWTUzSzpoeW9zdW5naXR4VGVzdC1zZWNyZXQteW1rcW5ZSk5sSWVDdmcwbnZHVkczYUI2cmhTZldQcWE=",
    "supportMobileApp": true,
    "support3rdParty": ["samsung", "lge", "skt"]
}


// complexConfigure


