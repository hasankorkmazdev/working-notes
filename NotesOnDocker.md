# Docker Notları



# Komutlar
Docker üzerinde oluşturduğun uygulama listesi (görüntüsü)

```sh 
docker image ls 
```

```ps 
docker build -t <docker-image-name> .
``` 
Docker build ile Image Oluşturma

`--no-cache` eğer rebuild işlemi yapıyorsanız bunu cache de aramamasını bildirmeniz gerekir.

## Docker Run
```ps
docker run -dp 5000:3000 <Docker Image Name> 
```
`-t` Image tagName 

`-p or -publish  5000:3000` 3000 Numaralı portu 5000 numaralı port olarak dışarıya aç

`-d or -detached` Terminalden bağımsız arka planda çalışmasını sağlar.

`--network <network-name>` Daha önceden tanımlanmış bir networkte çalışmasını sağlar

`--network-alias <short-name>` Çalışan uygulamanın networkdeki domain adı. 

`docker run` = `docker create` + `docker start.`



## Network
```ps 
docker network create <network-name>
```
 Docker engine üzerinde bir network oluşturmanızı sağlar.

## Docker Compose 

```ps
docker compose up -d
```
docker-compose.yml dosyasını ayağa kaldırır.

`-d or -detached` Terminalden bağımsız arka planda çalışmasını sağlar.

```ps
docker compose down
```
Docker Çalışan uygulamayı durdurur ve kaldırır

`--volume ` volumeler ile beraber kaldırır.

Uygulama yığınını sildiğinizde Docker Dashboard birimleri kaldırmaz.

```ps
docker compose stop
```
Docker Çalışan uygulamayı durdurur 

## Volume 

Docker'da volume oluşturulduğunda Windows'da fiziksel olarak adresde depolanır
``` ps 
cd "\\wsl$\docker-desktop-data\version-pack-data\community\docker\volumes\"
```

## Other 
```ps 
docker ps
```
`-a` Eğer bu komut kullanılmazsa sadece çalışan containerlar listelenir. 

Şuan aktif olarak çalışan container imageleri gösterir

```ps 
docker stop <container-id>
```
Bu komut çalışan bir uygulamayı durdurur. condainer-id için ilk 3 karakteri verebilirsin.

```ps 
docker rm -f  <container-id>
```
Çalışan container durdudur ve siler.
```ps 
docker rmi   <imagename>
```
Localdeki image siler

`-f` Eğer bu image bağlı bir container varsa reference hatası verir bu hatadan kurtulmak ve silmek için bu parametre kullanılmalı


```ps
docker logs -f <container-id>
```
Container içinde çalışan uygulamanın terminale bastığı 
çeriği izlersin.

`-f` Bu parametre kullanılmadığında docker anlık logları verir parametre kullanıldığında canlı akış halinde logları sağlar. 




## Sözlük
> Docker Desktop'ta çalışırken, Docker komutları aslında makinenizdeki küçük bir VM içinde çalışır. Mountpoint dizininin gerçek içeriğine bakmak istiyorsanız, önce VM'nin içine girmeniz gerekir.

> Docker Image Nedir ?

Bir container çalıştırırken, yalıtılmış bir dosya sistemi kullanır. Bu özel dosya sistemi bir docker image tarafından sağlanır . Docker Image dosya sistemini içerdiğinden, bir uygulamayı çalıştırmak için gereken her şeyi içermelidir - tüm bağımlılıklar, yapılandırma, komut dosyaları, ikili dosyalar, vb. Görüntü ayrıca kap için ortam değişkenleri, çalıştırılacak varsayılan bir komut, ve diğer meta veriler.
> Docker "Each container should do one thing and do it well"

Her container bir iş yapmalı onuda iyi yapmalı.

Bu yüzden tek bir container üzerine birden fazla uygulama veritabanı vs kurmamalıyız.

Hepsi ayrı bir container üzerinde çalışmalı 

> Docker Environment değişkenlerini Kullanmayı önermez !

Bağlantı ayarlarını ayarlamak için env değişkenlerini kullanmak genellikle geliştirme için uygun olsa da, uygulamaları üretimde çalıştırırken KESİNLİKLE KESİNLİKLE önerilmez . Docker'ın eski güvenlik lideri Diogo Monica, nedenini açıklayan harika bir blog yazısı yazdı .

Daha güvenli bir mekanizma, kapsayıcı düzenleme çerçeveniz tarafından sağlanan gizli desteği kullanmaktır. Çoğu durumda, bu gizli diziler, çalışan kapsayıcıda dosyalar olarak bağlanır. Birçok uygulamanın (MySQL görüntüsü ve yapılacaklar uygulaması dahil) değişkeni _FILEiçeren bir dosyaya işaret etmek için bir sonek ile env değişkenlerini desteklediğini göreceksiniz.

Örnek olarak, MYSQL_PASSWORD_FILEdeğişkenin ayarlanması, uygulamanın bağlantı parolası olarak başvurulan dosyanın içeriğini kullanmasına neden olur. Docker, bu env değişkenlerini desteklemek için hiçbir şey yapmaz. Uygulamanızın değişkeni aramayı ve dosya içeriğini almayı bilmesi gerekir.
