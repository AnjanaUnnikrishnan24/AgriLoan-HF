# AgriLoan-HF

## To build the network

```
./network.sh up createChannel -c autochannel -ca -s couchdb
```
## To add Org3 
```
cd addOrg3
```
```
./addOrg3.sh up -c autochannel -ca -s couchdb
```
```
cd ..
```

## To deploy the chaincode
```
 ./network.sh deployCC -ccn AgriSubsidy -ccp ../../AgriSubsidy/Chaincode/ -ccl go -c autochannel  -cccg ../../AgriSubsidy/Chaincode/collections.json
```
## General environment variable
```
export FABRIC_CFG_PATH=$PWD/../config/

export ORDERER_CA=${PWD}/organizations/ordererOrganizations/example.com/orderers/orderer.example.com/msp/tlscacerts/tlsca.example.com-cert.pem

export ORG1_PEER_TLSROOTCERT=${PWD}/organizations/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/ca.crt

export ORG2_PEER_TLSROOTCERT=${PWD}/organizations/peerOrganizations/org2.example.com/peers/peer0.org2.example.com/tls/ca.crt

export CORE_PEER_TLS_ENABLED=true
```
## ORG1




## PVT DATA

```
export CROPTYPE=$(echo -n "Paddy" | base64 | tr -d \n)
export LOCATION=$(echo -n "Kochi" | base64 | tr -d \n)
export LANDAREA=$(echo -n "56" | base64 | tr -d \n)
```
## Invoke

```
peer chaincode invoke \
  -o localhost:7050 \
  --ordererTLSHostnameOverride orderer.example.com \
  --tls \
  --cafile $ORDERER_CA \
  -C autochannel \
  -n AgriSubsidy \
  --peerAddresses localhost:7051 \
  --tlsRootCertFiles $ORG1_PEER_TLSROOTCERT \
  --peerAddresses localhost:9051 \
  --tlsRootCertFiles $ORG2_PEER_TLSROOTCERT \
  -c '{"Args":["SubsidyContract:ApplyForSubsidy","FID1234","ANIL","20","MACHINE PURCHASE","20000","558855478","2432IOB","IOB"]}' \
  --transient "{\"cropType\":\"${CROPTYPE}\",\"location\":\"${LOCATION}\",\"landArea\":\"${LANDAREA}\"}"
```

## Query

```
 peer chaincode query -C autochannel -n AgriSubsidy -c '{"Args":["SubsidyContract:ReadApplication","FID1234"]}'
```
## ORG2

invoke

```
peer chaincode invoke \
  -o localhost:7050 \
  --ordererTLSHostnameOverride orderer.example.com \
  --tls \
  --cafile $ORDERER_CA \
  -C autochannel \
  -n AgriSubsidy \
  --peerAddresses localhost:7051 \
  --tlsRootCertFiles $ORG1_PEER_TLSROOTCERT \
  --peerAddresses localhost:9051 \
  --tlsRootCertFiles $ORG2_PEER_TLSROOTCERT \
  -c '{"Args":["SubsidyContract:ApproveByAgri","FID1234","OFFICER_JOY"]}'

```

query
```
  peer chaincode query -C autochannel -n AgriSubsidy -c '{"Args":["SubsidyContract:ReadApplication","FID1234"]}'
```
