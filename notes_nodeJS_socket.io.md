# Socket.io

long-polling

used for real time interaction such as chat

connection stability depends on user

can be used in react redux

#### To set up connection

1. user need to login , get a token / UID , stored in DB
2. set up socket connection by sending token / UID , map socketID with UID , store in DB

#### On joining room

1. user send join room req
2. check DB for user socketID and room ID
3. socket join room , map room ID to UID

#### On disconnection

1. on disconnect , set up connect by sending token  / UID again (might need to check for window.close)
2. on re-connect , update socketID in DB
3. check if in room , join room accordingly