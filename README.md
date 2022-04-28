Here I post Links for State Sync Snapshots for the Cosmos Projects. And Also Guides how to use them.

Now there are projects:

- [Gravity Bridge](https://github.com/Phantom1605/State-Sync-Snapshots/blob/main/README.md#gravity-bridge)
- [Agoric](https://github.com/Phantom1605/State-Sync-Snapshots/blob/main/README.md#agoric) 


# Gravity Bridge
Stop Gravity and clear all uploaded blockchain data
```
sudo systemctl stop gravityd && gravity unsafe-reset-all
```

Specify Gravity RPC node as a variable
```
SNAP_RPC="http://194.233.93.124:26657"
```
Specify variables LATEST_HEIGHT, BLOCK_HEIGHT, TRUST_HASH
```
LATEST_HEIGHT=$(curl -s $SNAP_RPC/block | jq -r .result.block.header.height); \
BLOCK_HEIGHT=$((LATEST_HEIGHT - 1000)); \
TRUST_HASH=$(curl -s "$SNAP_RPC/block?height=$BLOCK_HEIGHT" | jq -r .result.block_id.hash); \
echo $LATEST_HEIGHT $BLOCK_HEIGHT $TRUST_HASH
```

Specify peer variable
```
peers="5f81c16ebeb7a74f81ca14d458f23b1595732034@194.233.93.124:26656"
```

Add peer to `persistent_peers`
```
sed -i -e "s/^persistent_peers *=.*/persistent_peers = \"$peers\"/" ~/.gravity/config/config.toml
```
Add SNAP_RPC, BLOCK_HEIGHT and TRUST_HASH in `config.toml`
```
sed -i -E "s|^(enable[[:space:]]+=[[:space:]]+).*$|\1true| ; \
s|^(rpc_servers[[:space:]]+=[[:space:]]+).*$|\1\"$SNAP_RPC,$SNAP_RPC\"| ; \
s|^(trust_height[[:space:]]+=[[:space:]]+).*$|\1$BLOCK_HEIGHT| ; \
s|^(trust_hash[[:space:]]+=[[:space:]]+).*$|\1\"$TRUST_HASH\"|" ~/.gravity/config/config.toml
```
Restart `gravityd` and see the logs
```
sudo systemctl restart gravityd && sudo journalctl -u gravityd -f --no-hostname -o cat
```



# Agoric
Stop Agoric and clear all uploaded blockchain data
```
sudo systemctl stop agoricd && ag0 unsafe-reset-all
```
Specify Agoric RPC node as a variable
```
SNAP_RPC="http://173.212.236.20:26657"
```
Specify variables LATEST_HEIGHT, BLOCK_HEIGHT, TRUST_HASH
```
LATEST_HEIGHT=$(curl -s $SNAP_RPC/block | jq -r .result.block.header.height); \
BLOCK_HEIGHT=$((LATEST_HEIGHT - 1000)); \
TRUST_HASH=$(curl -s "$SNAP_RPC/block?height=$BLOCK_HEIGHT" | jq -r .result.block_id.hash); \
echo $LATEST_HEIGHT $BLOCK_HEIGHT $TRUST_HASH
```
Specify peer variable
```
peers="f7b2caab145c6a766777420a2b4a0482ad375819@173.212.236.20:26656"
```
Add peer to `persistent_peers`
```
sed -i -e "s/^persistent_peers *=.*/persistent_peers = \"$peers\"/" ~/.agoric/config/config.toml
```
Add SNAP_RPC, BLOCK_HEIGHT and TRUST_HASH in `config.toml`
```
sed -i -E "s|^(enable[[:space:]]+=[[:space:]]+).*$|\1true| ; \
s|^(rpc_servers[[:space:]]+=[[:space:]]+).*$|\1\"$SNAP_RPC,$SNAP_RPC\"| ; \
s|^(trust_height[[:space:]]+=[[:space:]]+).*$|\1$BLOCK_HEIGHT| ; \
s|^(trust_hash[[:space:]]+=[[:space:]]+).*$|\1\"$TRUST_HASH\"|" $HOME/.agoric/config/config.toml
```
Restart `agoricd` and see the logs
```
sudo systemctl restart agoricd && sudo journalctl -u agoricd -f --no-hostname -o cat
```



