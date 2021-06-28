# Steps to upgrade cardano node from 1.26 to 1.27

## download cardano-node latest repo
```
git clone https://github.com/input-output-hk/cardano-node.git cardano-node-1.27
```

## build and update cabal and cardano
```
cd cardano-node-1.27
cabal update
git fetch --all --recurse-submodules --tags
git checkout $(curl -s https://api.github.com/repos/input-output-hk/cardano-node/releases/latest | jq -r .tag_name)
cabal configure -O0 -w ghc-8.10.4
echo -e "package cardano-crypto-praos\n flags: -external-libsodium-vrf" > cabal.project.local
cabal build cardano-node cardano-cli
```

## verify upgrade
```
$(find $HOME/git/cardano-node-1.27/dist-newstyle/build -type f -name "cardano-cli") version
$(find $HOME/git/cardano-node-1.27/dist-newstyle/build -type f -name "cardano-node") version
```

## Stop cardano node
```
sudo systemctl stop cardano-node
```

## Copy 1.27 to bin and verify version
```
sudo cp $(find $HOME/git/cardano-node-1.27/dist-newstyle/build -type f -name "cardano-cli") /usr/local/bin/cardano-cli
sudo cp $(find $HOME/git/cardano-node-1.27/dist-newstyle/build -type f -name "cardano-node") /usr/local/bin/cardano-node
cardano-node version
cardano-cli version
```

## Start the node and verify everything works well
```
sudo systemctl start cardano-node
```
