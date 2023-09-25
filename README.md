## 1: Öncelikle sistemdeki tüm container, image ve volumeleri görelim. Bunun için ayrı ayrı listeleme komutlarını girelim. 

docker container ls
docker image ls
docker volume ls

Ve ardından temizlik yapmak adına makinenizdeki tüm containerları, imageleri ve volumeleri temizleyelim. Bunun iki yöntemi var. Bakalım siz kolay olanı mı seçeceksiniz. 

docker container prune -a
docker image prune -a
docker volume prune -a


## 2: centos, alpine, nginx, httpd:alpine, ozgurozturknet/adanzyedocker, ozgurozturknet/hello-app, ozgurozturknet/app1 isimli imajları çalıştığımız sisteme çekelim. 

docker image pull centos
docker image pull alpine
docker image pull nginx
docker image pull httpd:alpine
docker image pull ozgurozturknet/adanzyedocker
docker image pull ozgurozturknet/hello-app
docker image pull ozgurozturknet/app1

## 3: ozgurozturknet/app1 isimli imajdan bir container yaratalım.

docker container run --name app1 ozgurozturknet/app1

## 4: httpd:alpine isimli imajdan detached bir container yaratalım. Yarattığımız container ismini ve id’sini görelim. 

docker container run -d httpd:alpine
docker container ls

## 5: Yarattığımız bu contaier’ın loglarına bakalım.

docker container logs nervous_turing

## 6: Container’ı durduralım, ardından yeniden çalıştıralım ve son olarak container’ı sistemden kaldıralım. 

docker container stop nervous_turing
docker container start nervous_turing
docker container rm -f nervous_turing

## 7: ozgurozturknet/adanzyedocker isimli imajdan websunucu adında detached ve “-p 80:80” ile portu publish edilmiş bir container yaratalım. Kendi bilgisayarımızın browserından bu web sitesine erişelim.

docker container run -d -p 80:80 --name websunucu ozgurozturknet/adanzyedocker

## 8: websunucu adlı bu container’ın içerisine bağlanalım. /usr/local/apache2/htdocs klasörünün altına geçelim ve echo “denemedir” >> index.html komutuyla buradaki dosyaya denemedir yazısını ekleyelim. Web tarayıcıya geçerek dosyaya ekleme yapabildiğimizi görmek için refresh edelim. Sonrasında container içerisinden exit ile çıkalım.

docker exec -it websunucu sh 
cd /usr/local/apache2/htdocs/
echo "denemedir" >> index.html 
exit

## 9: websunucu isimli container’ı çalışırken silelim.

docker container rm -f websunucu

## 10: alpine isimli imajdan bir container yaratalım. Ama varsayılan olarak çalışması gereken uygulama yerine “ls” uygulamasının çalışmasını sağlayalım.

docker container run alpine ls

## 11: “alistirma1” isimli bir volüme yaratalım. 

docker volume create alistirma1

## 12: alpine isimli imajdan “birinci” isimli bir container yaratalım. Bu container’ı interactive modda yaratalım ve bağlanabilelim. Aynı zamanda “alistirma1” isimli volume’u bu containerın “/test” isimli folder’ına mount edelim. Bu folder içerisine geçelim ve “touch abc.txt” komutuyla bir dosya yaratalım daha sonra “echo deneme >> abc.txt” komutuyla bu dosyanın içerisine yazı ekleyelim. 

docker container run -it --name birinci -v alistirma1:/test alpine sh
cd test/
touch abc.txt
ls
echo "deneme" >> abc.txt


## 13: alpine isimli imajdan “ikinci” isimli bir container yaratalım. Bu container’ı interactive modda yaratalım ve bağlanabilelim. Aynı zamanda “alistirma1” isimli volume’u bu containerın “/test” isimli folder’ına mount edelim. Bu folder içerisinde “ls” komutyla dosyaları listeleyelim ve abc.txt dosyası olduğunu görelim. “cat abc.txt” ile dosyanın içeriğini kontrol edelim. 

docker container run -it --name ikinci -v alistirma1:/test alpine sh
cd test/
ls
cat abc.txt

## 14: alpine isimli imajdan “ucuncu” isimli bir container yaratalım. Bu container’ı interactive modda yaratalım ve bağlanabilelim. Aynı zamanda “alistirma1” isimli volume’u bu containerın “/test” isimli folder’ına mount edelim fakat Read Only olarak mount edelim. Bu folder içerisine geçelim ve “touch abc1.txt” komutuyla bir dosya yaratmaya çalışalım. Ve yaratamadığımızı görelim.

docker container run -it --name ucuncu -v alistima:/test:ro alpine sh
cd test/
touch abc1.txt
"touch: abc1.txt: Read-only file system"

## 15: Bilgisayarımızda bir klasör yaratalım “örneğin c:\deneme” ve bu klasörün içerisinde index.html adlı bir dosya yaratıp bu dosyanın içerisine birkaç yazı ekleyelim.

mkdir deneme
cd deneme
touch index.html
echo "alo ordamisin" >> index.html
echo "rahatta misin" >> index.html
pwd
/Users/oboyraz/Documents/deneme

## 16: ozgurozturknet/adanzyedocker isimli imajdan websunucu1 adında detached ve “-p 80:80” ile portu publish edilmiş bir container yaratalım. Bilgisayarımızda yarattığımız klasörü container’ın içerisindeki klasörüne mount edelim. Web browser açarak 127.0.0.1’e gidelim ve sitemizi görelim. Daha sonra bilgisayarımızda yarattığımız klasörün içerisindeki index.html dosyasını edit edelim ve yeni yazılar ekleyelim. Web sayfasını refresh ederek bunların geldiğini görelim.

docker container run -d -p 80:80 --name websunucu1 -v /Users/oboyraz/Documents/deneme:/usr/local/apache2/htdocs ozgurozturknet/adanzyedocker

echo "bir basina uzakta misin"

## 17: Tüm çalışan container’ları silelim. 

docker container prune
docker container rm -f websunucu1
