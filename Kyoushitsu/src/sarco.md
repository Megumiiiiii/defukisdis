# Sarcophagus

### Official Links
- [Twitter](https://twitter.com/sarcophagusio)
- [Discord](https://discord.gg/sarcophagus-community-753398645507883099)
- [Website](https://sarcophagus.io/)


## Minimum requirements

| Spec | Size |
|------|------|
| 20GB | SSD |
|      |     |
| 2GB  | RAM |

## Prerequisites

- Git
- [Dokcer >= 20.0](https://www.simplilearn.com/tutorials/docker-tutorial/how-to-install-docker-on-ubuntu)
- [Dokcer compose >= 2.0.0)](https://docs.docker.com/compose/install/linux/#install-the-plugin-manually)
- Server harus memiliki port 80 dan 443 terbuka
- ETH wallet ( Private Key & Mnemonic )
    - ETH/Matic balance (untuk sign transaksi)
    - SARCO balance (for bonding your archaeologist to curses)
- RPC wss:// URL (Infura, Alchemy, etc.)
- Domain yang menunjuk ke alamat IP server Anda

### Setup DNS Record

- Buka pengelolaan domain
- Tambahkan Catatan DNS.
- Pilih **A**.
- Masukkan nama Anda dan alamat IP publik Anda sebagai nilai.
- Simpan
  
## Install Dependencies
```bash
sudo apt update; sudo apt upgrade
```
```bash
sudo apt-get update && sudo apt install git -y && sudo apt install apt-transport-https ca-certificates curl software-properties-common -y && curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add - && sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu focal stable" && sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin -y
```

### Open Port
```bash
sudo ufw allow ssh; sudo ufw allow 443/tcp; sudo ufw allow 80/tcp; sudo ufw enable
```

## Clone Repo
```bash
git clone https://github.com/sarcophagus-org/quickstart-archaeologist
cd quickstart-archaeologist
```

### Copy `.env.sample` to `.env`
```bash
cp .env.example .env
```

### Generate Mnemonic Pharse

- [Here](https://iancoleman.io/bip39/)
- or

```
COMPOSE_PROFILES=seed-gen docker compose run seed-gen
```

**BACKUP**

### Buat file kosong  `peer-id.json`

```bash
touch peer-id.json
```

### Edit `.env` file
```bash
nano .env
```
Fill in your data


1. Gunakan wss:// bukan https:// untuk `PROVIDER_URL`
2. Butuh `ENCRYPTION_MNEMONIC` yang berbeda untuk setiap chain, generate [Here](https://iancoleman.io/bip39/) atau dimanapun
3. `ETH_PRIVATE_KEY` Private key yang berisi ETH/Matic dan $SARCO
4. `NOTIFICATION_WEBHOOK_URL` adalah discord webhook URL. Bisa baca disini untuk cara mengaturnya [discord webhook url](https://support.discord.com/hc/en-us/articles/228383668-Intro-to-Webhooks) (Optional)


- Example ↓ ↓ ↓

```rust,editable
## Key used to encrypt secrets and sign ETH transactions
## The address associated with this key is your archaeologist identifier
ETH_PRIVATE_KEY=ugfhttawfihqwoid132b1231b23

## (optional) multiply gas price estimation by this amount (i.e. 2 means 2x RPC gas price estimate)
## GAS_MULTIPLIER=
NOTIFICATION_WEBHOOK_URL=https://discord.com/api/webhooks/hafkakakjajbasbkadsbaksdjbakfbakfaf

## Domain to use for your archaeologist
## This domain should be pointed with an A record to your server's IP
DOMAIN=my.exampledomain.com

## Comma-separated list of chain ids of each network you want to run your service on.
## Supported Chain IDS:
## 1 = Mainnet
## 5 = Goerli
## 11155111 = Sepolia
## 84531 = Base Goerli
## 80001 = Polygon Mumbai
## example, to run on mainnet, goerli, and sepolia, set:
## CHAIN_IDS="1,5,11155111"
CHAIN_IDS=1,42161,137

## Here, you should define:
## - RPC provider urls.
## - Mnemonics used to derive keypairs to encrypt sarcophagi.
##   Generate a new one here - https://iancoleman.io/bip39/ or see README to generate one offline.
## Uncomment and set, for each network chain id you want to run on, your own
## private provider URL (infura, alchemy, etc) and a unique mnemonic for that network.
## ================================================================================================
MAINNET_PROVIDER_URL=wss://eth-mainnet
MAINNET_ENCRYPTION_MNEMONIC=never gonna give you up never gonna let you down

ARBITRUM_PROVIDER_URL==wss://arb-mainnet
ARBITRUM_ENCRYPTION_MNEMONIC=never gonna tell a lie and hurt you

POLYGON_MAINNET_PROVIDER_URL=wss://polygon-mainnet
POLYGON_MAINNET_ENCRYPTION_MNEMONIC=never gonna tell a lie and hurt you
```



### $SARCO Token
- Untuk mendapatkan $SARCO, Anda dapat menukarkannya di Uniswap
- Go to [Uniswap ETH](https://app.uniswap.org/tokens/ethereum/0x7697b462a7c4ff5f8b55bdbc2f4076c2af9cf51a)
- Go to [Uniswap Polygon](https://app.uniswap.org/tokens/polygon/0x80ae3b3847e4e8bd27a389f7686486cac9c3f3e8)
- Go to [Uniswap Arbitrum](https://app.uniswap.org/tokens/arbitrum/0x82155Ab6b6c1113CFb352c7573B010a88f5974bD)
- SC Sarco Ethereum: `0x7697B462A7c4Ff5F8b55BDBC2F4076c2aF9cF51A`
- SC Sarco Polygon: `0x80ae3b3847e4e8bd27a389f7686486cac9c3f3e8`
- SC Sarco Arbitrum: `0x82155Ab6b6c1113CFb352c7573B010a88f5974bD`

## Register

Jika ingin run di 3 chain, maka perlu register 1 by 1.

```bash
COMPOSE_PROFILES=register NETWORK=mainnet docker compose run register
```
- Y, Enter
- Then enter the nominal ( recommended: DiggingFee 100 - 500, CurseFee 300, and FreeBond 1000 )

Ouput:
```bash
=========================================================================================================

ARCHAEOLOGIST PROFILE: 

FIELD                      VALUE                                                                       
exists                     true                                                                        
maximumRewrapInterval      200 days (17280000s)                                                        
maximumResurrectionTime    Dec 30 2023 (1703953499)                                                    
peerId                     sarcophagus.example.xyz:12D3KooWRkyaFVBDFaaf3D5piG1YRJjCMSgBMj9Si4xjFDaRqjSCX
minimumDiggingFeePerSecond 0.000153712226361799 SARCO (~ 400.00/month)                                 
freeBond                   1000.0SARCO                                               
cursedBond                 0 SARCO                                                
curseFee                   300.0 SARCO                                                                 
address                    0x897015991ABC646a69EC8701B8459aA806aCf70a                                  

=========================================================================================================
```

### Start node
```bash
COMPOSE_PROFILES=service NETWORK=all docker compose up -d
```


## Useful Command 

#### Check Archaeologist Service's logs

```bash
docker logs -f quickstart-archaeologist-archaeologist-1
```

#### Check Acme Companion's logs

```bash
docker logs -f  quickstart-archaeologist-acme-companion-1
```

#### Check Nginx's logs

```bash
docker logs -f nginx-proxy
```
	
#### Updating the service

```bash
cd ~/quickstart-archaeologist
git pull
COMPOSE_PROFILES=service NETWORK=all docker compose down
COMPOSE_PROFILES=service NETWORK=all docker compose pull
COMPOSE_PROFILES=service NETWORK=all docker compose up -d
```

#### Restarting node

```bash
COMPOSE_PROFILES=service NETWORK=all docker compose restart
```

#### Update domain




```
NETWORK=all docker compose exec -it archaeologist sh
cli update -u -n mainnet
```

```
exit
```

#### Updating Digging Fee

```bash
NETWORK=all docker compose exec -it archaeologist sh
cli update -d 300 -n mainnet
```

```bash
exit
```
300 dapat diubah menjadi jumlah yang lain

#### Updating Curse Fee

```bash
NETWORK=all docker compose exec -it archaeologist sh
cli update -c 300 -n mainnet
```

```bash
exit
```
300 dapat diubah menjadi jumlah yang lain


#### Updating Profile

```bash
NETWORK=all docker compose exec -it archaeologist sh
cli update -u -n mainnet
```

```bash
exit
```

#### Updating Free Bond 

```bash
NETWORK=all docker compose exec -it archaeologist sh
cli update -f 100 -n mainnet
```

```bash
exit
```
100 dapat diubah menjadi jumlah yang lain


#### CheckProfile

```bash
NETWORK=all docker compose exec -it archaeologist sh
cli view -p -n mainnet
```

```bash
exit
```

#### Claim Rewards

```bash
NETWORK=all docker compose exec -it archaeologist sh
cli claim -n mainnet
```

```bash
exit
```

#### Withdraw Free Bond

```bash
NETWORK=all docker compose exec -it archaeologist sh
cli free-bond -w 10 -n mainnet
```

```bash
exit
```
10 bisa dubah berapapun

#### any other

```bash
NETWORK=all docker compose exec -it archaeologist sh
cli help -n mainnet
```

```bash
exit
```

### Troubleshooting

- Domain A Record

Domain Anda harus memiliki catatan A yang menunjuk ke alamat IP server di mana layanan arkeolog dijalankan.[NsLookup](https://www.nslookup.io/website-to-ip-lookup) Gunakan alat ini untuk memastikan bahwa domain Anda sudah benar-benar diarahkan.

- Test Websocket Connection

[Piesocket](https://www.piesocket.com/websocket-tester) Uji apakah Arkeolog Anda dapat membuka koneksi websocket dengan memasukkan alamat websocket Anda dalam format ini:
```
wss://your.domain/p2p/yourPeerID
```
Untuk mendapatkan domain dan peerID Anda, jalankan: [Check Profile](#checkprofile)

#### ⚠️ If you want to delete ⚠️

```bash 
cd ~/quickstart-archaeologist
COMPOSE_PROFILES=service docker compose down -v
docker rmi jwilder/nginx-proxy nginxproxy/acme-companion ghcr.io/sarcophagus-org/sarcophagus-v2-archaeologist-service
```
