
CPU: 4 cores / 8 threads, 2.6GHz, or faster
RAM: 8GB or more
Disk: 500GB of SSD storage or more
Network: 1GBps, Public IP


## Update edelim
```bash
sudo apt update; sudo apt upgrade 
```
## Docker kurulumu
```bash
sudo apt-get update && sudo apt install jq git && sudo apt install apt-transport-https ca-certificates curl software-properties-common -y && curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add - && sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu focal stable" && sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin && sudo apt-get install docker-compose-plugin 

```
## Dosyaları çekelim
```
docker pull arthera/arthera-node:latest
```
## Dosya oluşturalım
```
mkdir $HOME/arthera
```
## Cüzdan oluşturalım
```
docker run -it -v $HOME/arthera:/data arthera/arthera-node:latest account new
```
👉Bizden şifre vs isteyecek daha sonra aşağıdaki gibi bir çıktı alıcaz kaydedelim :

-----------------
![image](https://github.com/molla202/Arthera-Testnet/assets/91562185/1e261c1d-7e4c-4438-b29f-e6524c9a07ae)

-----------------

👉oluşan key dosyasını kendi pcömize yedekliyelim $HOME/arthera/keystore/UTC-**** ( evet winscp gibi bişile bağlanıp alıcaz)
👉şifrenizi kaydettiğinizden emin olun

## Cüzdanınıza bakiye alın. sanırım discordan form doldurup rol alanlar için bir kanal var.
500,000 AA coins lazım

## Validator oluşturalım.
```
docker run -it -v $HOME/arthera:/data arthera/arthera-node:latest validator new
```
👉şifre falan sorucak giricez alttaki gibi çıktı alıcaz kaydedin:

---------------------------------
![image](https://github.com/molla202/Arthera-Testnet/assets/91562185/7963f022-0e8f-4acc-89f1-8631fdc6f6d0)

-----------------------------------------

👉oluşan validator dosyamızı kendi pcmize yedekliyelim $HOME/arthera/keystore/validator
👉çıkan bilgileri kaydetmeyi unutmayın
👉şifrenizi unutmayın

-----------------------------------

## Validator kayıt işlemleri

👉ilk basta oluşturduğumuz ve yedeklediğimiz cüzdan dosyasını mm cüzdanımıza json olarak ekliyoruz
👉adresine gidelim https://wallet-test.arthera.net
👉mm ile bağnalım
👉Arthera Wallet'ta Hesap menü öğesini seçin
👉Doğrulayıcı Olun bölümünün altında, validator oluşturun kısmında çıkan çıktıda yazan public keyi girin. Doğrulayıcı kimliğini oluşturun
👉kayıt olun
👉ve bize bir validator ID vericek kaydedin önemli.

------------------------------------------



## Şifre oluşturalım

aşağıdaki komutu çalıştıralım. validator oluşturalım kısmında yazdığımız şifreyi yazalım YOUR_PASSWORD yerine

```
echo "YOUR_PASSWORD" > $HOME/arthera/keystore/validator/password
```
-----------------------------------------------
## Nodu başlatalım

👉Aşağıdaki değişkenleri değiştirelim :
👉YOUR_PUBLIC_IP : sunucu ip adresiniz.
👉ID_FROM_STEP_8 : validator kayıt işlemi sonunda aldığımız bir ID vardı onu yazıyoruz.
👉VALIDATOR_PUBLIC_KEY_FROM_STEP_7 : validator oluşturalım kısmındaki çıkan çıktıdaki public keyi yazalım.

-----------------------------------------
```
docker run -v $HOME/arthera:/data arthera/arthera-node:latest \
  --testnet \
  --nat extip:YOUR_PUBLIC_IP \
  --validator.id ID_FROM_STEP_8 \
  --validator.pubkey "VALIDATOR_PUBLIC_KEY_FROM_STEP_7" \
  --validator.password "/data/keystore/validator/password"
```
