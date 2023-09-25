## 1: Öncelikle sistemdeki tüm container, image ve volumeleri görelim. Bunun için ayrı ayrı listeleme komutlarını girelim. 

```bash
docker container ls
```
```bash
docker image ls
```
```bash
docker volume ls
```

## Ve ardından temizlik yapmak adına makinenizdeki tüm containerları, imageleri ve volumeleri temizleyelim. Bunun iki yöntemi var. Bakalım siz kolay olanı mı seçeceksiniz. 

```bash
docker container prune -a
```
```bash
docker image prune -a
```
```bash
docker volume prune -a
```

## 2: centos, alpine, nginx, httpd:alpine, ozgurozturknet/adanzyedocker, ozgurozturknet/hello-app, ozgurozturknet/app1 isimli imajları çalıştığımız sisteme çekelim. 

```bash
docker image pull centos
```
```bash
docker image pull alpine
```
```bash
docker image pull nginx
```
```bash
docker image pull httpd:alpine
```
```bash
docker image pull ozgurozturknet/adanzyedocker
```
```bash
docker image pull ozgurozturknet/hello-app
```
```bash
docker image pull ozgurozturknet/app1
```

## 3: ozgurozturknet/app1 isimli imajdan bir container yaratalım.

```bash
docker container run --name app1 ozgurozturknet/app1
```

## 4: httpd:alpine isimli imajdan detached bir container yaratalım. Yarattığımız container ismini ve id’sini görelim. 

```bash
docker container run -d httpd:alpine
```
```bash
docker container ls
```

## 5: Yarattığımız bu contaier’ın loglarına bakalım.

```bash
docker container logs nervous_turing
```

## 6: Container’ı durduralım, ardından yeniden çalıştıralım ve son olarak container’ı sistemden kaldıralım. 

```bash
docker container stop nervous_turing
```
```bash
docker container start nervous_turing
```
```bash
docker container rm -f nervous_turing
```

## 7: ozgurozturknet/adanzyedocker isimli imajdan websunucu adında detached ve “-p 80:80” ile portu publish edilmiş bir container yaratalım. Kendi bilgisayarımızın browserından bu web sitesine erişelim.

```bash
docker container run -d -p 80:80 --name websunucu ozgurozturknet/adanzyedocker
```

## 8: websunucu adlı bu container’ın içerisine bağlanalım. /usr/local/apache2/htdocs klasörünün altına geçelim ve echo “denemedir” >> index.html komutuyla buradaki dosyaya denemedir yazısını ekleyelim. Web tarayıcıya geçerek dosyaya ekleme yapabildiğimizi görmek için refresh edelim. Sonrasında container içerisinden exit ile çıkalım.

```bash
docker exec -it websunucu sh
```
```bash
cd /usr/local/apache2/htdocs/
```
```bash
echo "denemedir" >> index.html 
```
```bash
exit
```

## 9: websunucu isimli container’ı çalışırken silelim.

```bash
docker container rm -f websunucu
```

## 10: alpine isimli imajdan bir container yaratalım. Ama varsayılan olarak çalışması gereken uygulama yerine “ls” uygulamasının çalışmasını sağlayalım.

```bash
docker container run alpine ls
```

## 11: “alistirma1” isimli bir volume yaratalım. 

```bash
docker volume create alistirma1
```

## 12: alpine isimli imajdan “birinci” isimli bir container yaratalım. Bu container’ı interactive modda yaratalım ve bağlanabilelim. Aynı zamanda “alistirma1” isimli volume’u bu containerın “/test” isimli folder’ına mount edelim. Bu folder içerisine geçelim ve “touch abc.txt” komutuyla bir dosya yaratalım daha sonra “echo deneme >> abc.txt” komutuyla bu dosyanın içerisine yazı ekleyelim. 

```bash
docker container run -it --name birinci -v alistirma1:/test alpine sh
```
```bash
cd test/
```
```bash
touch abc.txt
```
```bash
ls
```
```bash
echo "deneme" >> abc.txt
```

## 13: alpine isimli imajdan “ikinci” isimli bir container yaratalım. Bu container’ı interactive modda yaratalım ve bağlanabilelim. Aynı zamanda “alistirma1” isimli volume’u bu containerın “/test” isimli folder’ına mount edelim. Bu folder içerisinde “ls” komutyla dosyaları listeleyelim ve abc.txt dosyası olduğunu görelim. “cat abc.txt” ile dosyanın içeriğini kontrol edelim. 

```bash
docker container run -it --name ikinci -v alistirma1:/test alpine sh
```
```bash
cd test/
```
```bash
ls
```
```bash
cat abc.txt
```

## 14: alpine isimli imajdan “ucuncu” isimli bir container yaratalım. Bu container’ı interactive modda yaratalım ve bağlanabilelim. Aynı zamanda “alistirma1” isimli volume’u bu containerın “/test” isimli folder’ına mount edelim fakat Read Only olarak mount edelim. Bu folder içerisine geçelim ve “touch abc1.txt” komutuyla bir dosya yaratmaya çalışalım. Ve yaratamadığımızı görelim.

```bash
docker container run -it --name ucuncu -v alistima:/test:ro alpine sh
```
```bash
cd test/
```
```bash
touch abc1.txt
```
```bash
"touch: abc1.txt: Read-only file system"
```

## 15: Bilgisayarımızda bir klasör yaratalım “örneğin c:\deneme” ve bu klasörün içerisinde index.html adlı bir dosya yaratıp bu dosyanın içerisine birkaç yazı ekleyelim.

```bash
mkdir deneme
```
```bash
cd deneme
```
```bash
touch index.html
```
```bash
echo "deneme1" >> index.html
```
```bash
echo "deneme2" >> index.html
```
```bash
pwd
```
```bash
/Users/oboyraz/Documents/deneme
```

## 16: ozgurozturknet/adanzyedocker isimli imajdan websunucu1 adında detached ve “-p 80:80” ile portu publish edilmiş bir container yaratalım. Bilgisayarımızda yarattığımız klasörü container’ın içerisindeki klasörüne mount edelim. Web browser açarak 127.0.0.1’e gidelim ve sitemizi görelim. Daha sonra bilgisayarımızda yarattığımız klasörün içerisindeki index.html dosyasını edit edelim ve yeni yazılar ekleyelim. Web sayfasını refresh ederek bunların geldiğini görelim.

```bash
docker container run -d -p 80:80 --name websunucu1 -v /Users/oboyraz/Documents/deneme:/usr/local/apache2/htdocs ozgurozturknet/adanzyedocker
```
```bash
echo "deneme3" >> index.html
```

## 17: Tüm çalışan container’ları silelim. 

```bash
docker container prune
```
```bash
docker container rm -f websunucu1
```
