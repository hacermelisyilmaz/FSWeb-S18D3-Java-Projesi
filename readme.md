# SQL Sorgu Alıştırmaları

Bu hafta SQL sorguları üzerine çalışıyorsunuz. Bugünkü alıştırmada sizin için hazırladığımız veritabanında aşağıda istediğimiz sonuçları elde etmenize yarayan SQL sorgularını oluşturacaksınız.

## Proje Kurulumu
Projeyi forklayın ve clonlayın. Tamamladığınızda da pushlayın.

### Kütüphane Bilgi Sistemi

Bu veritabanı, bir okulun kütüphanesinden öğrencilerin aldıkları kitapların bilgisini barındırmaktadır.

#### Tablolar 
`ogrenci` tablosu öğrencilerin listesini tutar.
`islem` tablosu öğrencilerin kütüphaneden aldıkları kitapların bilgilerini tutar
`kitap` tablosu kütüphanedeki kitapların bilgisini tutar
`yazar` tablosu kitapların yazarları bilgisini tutar
`tur` tablosu kitapların türlerini tutar.

Tablo ilişiklerini görmek için [ktphn.png] dosyasına göz atın.

Yazdığınız sorguları buradan test edebilirsiniz: [https://ergineer.com/assets/materials/fkg36so5-kutuphanebilgisistemi-sql/]


##### Görevler
Aşağıda istenilen sonuçlara ulaşabilmek için gerekli SQL sorgularını yazın. 


MIN-MAX, COUNT-AVG-SUM, GROUP BY, JOINS (INNER, OUTER, LEFT, RIGHT
	#ilk 3 soruyu join kullanmadan yazın.
	1) Öğrencinin adını, soyadını ve kitap aldığı tarihleri listeleyin.
	
		SELECT ogrenci.ograd, ogrenci.ogrsoyad, islem.atarih FROM ogrenci, islem WHERE ogrenci.ogrno = islem.ogrno
	
	2) Fıkra ve hikaye türündeki kitapların adını ve türünü listeleyin.
			
		SELECT kitap.kitapadi, tur.turadi FROM kitap, tur WHERE tur.turadi = "Fıkra" OR tur.turadi = "Hikaye"
	
	3) 10B veya 10C sınıfındaki öğrencilerin numarasını, adını, soyadını ve okuduğu kitapları listeleyin.

		SELECT ogrenci.ogrno, ogrenci.ograd, ogrenci.ogrsoyad, kitap.kitapadi FROM ogrenci, kitap, islem WHERE kitap.kitapno = islem.kitapno AND ogrenci.ogrno = islem.ogrno AND ogrenci.sinif IN("10B", "10C")
	
	#join ile yazın
	4) Öğrencinin adını, soyadını ve kitap aldığı tarihleri listeleyin.
	
		SELECT ogrenci.ograd, ogrenci.ogrsoyad, islem.atarih FROM ogrenci JOIN islem ON ogrenci.ogrno = islem.ogrno
	
	5) Fıkra ve hikaye türündeki kitapların adını ve türünü listeleyin.

		SELECT kitap.kitapadi, tur.turadi FROM kitap JOIN tur ON tur.turadi = "Fıkra" OR tur.turadi = "Hikaye"

	6) 10B veya 10C sınıfındaki öğrencilerin numarasını, adını, soyadını ve okuduğu kitapları, öğrenci adına göre listeleyin.
	
		SELECT ogrenci.ogrno, ogrenci.ograd, ogrenci.ogrsoyad, kitap.kitapadi FROM islem JOIN kitap ON kitap.kitapno = islem.kitapno JOIN ogrenci ON ogrenci.ogrno = islem.ogrno AND ogrenci.sinif IN("10B", "10C")
	
	7) Kitap alan öğrencinin adı, soyadı, kitap aldığı tarih listelensin. Kitap almayan öğrencilerinde listede görünsün.
	
		SELECT ogrenci.ograd, ogrenci.ogrsoyad, islem.atarih FROM ogrenci LEFT JOIN islem ON ogrenci.ogrno = islem.ogrno
	
	8) Kitap almayan öğrencileri listeleyin.
	
		SELECT ogrenci.ograd, ogrenci.ogrsoyad FROM ogrenci LEFT JOIN islem ON ogrenci.ogrno = islem.ogrno WHERE islem.atarih IS NULL
	
	9) Alınan kitapların kitap numarasını, adını ve kaç defa alındığını kitap numaralarına göre artan sırada listeleyiniz.
	
		SELECT kitap.kitapno, kitap.kitapadi, COUNT(islem.kitapno) AS islemsayisi FROM kitap LEFT JOIN islem ON kitap.kitapno = islem.kitapno GROUP BY kitap.kitapno, kitap.kitapadi ORDER BY kitap.kitapno ASC
	
	10) Alınan kitapların kitap numarasını, adını kaç defa alındığını (alınmayan kitapların yanında 0 olsun) listeleyin.

		SELECT kitap.kitapno, kitap.kitapadi, COUNT(islem.kitapno) AS islem_sayisi FROM kitap LEFT JOIN islem ON kitap.kitapno = islem.kitapno GROUP BY kitap.kitapno, kitap.kitapadi

	11) Öğrencilerin adı soyadı ve aldıkları kitabın adı listelensin.
	
		SELECT ogrenci.ograd, ogrenci.ogrsoyad, kitap.kitapadi FROM islem JOIN ogrenci ON islem.ogrno = ogrenci.ogrno JOIN kitap ON kitap.kitapno = islem.kitapno
	
	12) Her öğrencinin adı, soyadı, kitabın adı, yazarın adı soyad ve kitabın türünü ve kitabın alındığı tarihi listeleyiniz. Kitap almayan öğrenciler de listede görünsün.
	
		SELECT ogrenci.ograd, ogrenci.ogrsoyad, kitap.kitapadi, yazar.yazarad, yazar.yazarsoyad, tur.turadi 
			FROM islem
			JOIN ogrenci ON islem.ogrno = ogrenci.ogrno
			JOIN kitap ON islem.kitapno = kitap.kitapno
			JOIN yazar ON kitap.yazarno = yazar.yazarno
			JOIN tur ON kitap.turno = tur.turno
	
	13) 10A veya 10B sınıfındaki öğrencilerin adı soyadı ve okuduğu kitap sayısını getirin.
	
		SELECT ogrenci.ograd, ogrenci.ogrsoyad, COUNT(islem.islemno) AS numberofbooksread FROM ogrenci 
			JOIN islem ON islem.ogrno = ogrenci.ogrno
			WHERE ogrenci.sinif IN("10A", "10B")
			GROUP BY ogrenci.ograd, ogrenci.ogrsoyad
	
	14) Tüm kitapların ortalama sayfa sayısını bulunuz. #AVG

		SELECT AVG(kitap.sayfasayisi) as ortalamasayfa FROM kitap
	
	15) Sayfa sayısı ortalama sayfanın üzerindeki kitapları listeleyin.
	
		SELECT kitap.kitapno, kitap.kitapadi, kitap.sayfasayisi, AVG(kitap.sayfasayisi) FROM kitap
			GROUP BY kitap.kitapno, kitap.kitapadi, kitap.sayfasayisi
			HAVING kitap.sayfasayisi > AVG(kitap.sayfasayisi)
	
	16) Öğrenci tablosundaki öğrenci sayısını gösterin
	
		SELECT COUNT(ogrenci.ogrno) FROM ogrenci
	
	17) Öğrenci tablosundaki toplam öğrenci sayısını toplam sayı takma(alias kullanımı) adı ile listeleyin.
	
		SELECT COUNT(ogrenci.ogrno) AS toplamsayi FROM ogrenci
	
	18) Öğrenci tablosunda kaç farklı isimde öğrenci olduğunu listeleyiniz.
	
		SELECT COUNT(DISTINCT ogrenci.ograd) AS uniqueogrencisayisi FROM ogrenci
	
	19) En fazla sayfa sayısı olan kitabın sayfa sayısını listeleyiniz.
	
		SELECT MAX(kitap.sayfasayisi) AS maxsayfasayisi FROM kitap
	
	20) En fazla sayfası olan kitabın adını ve sayfa sayısını listeleyiniz.
	
		SELECT kitapadi, sayfasayisi FROM kitap WHERE sayfasayisi = (SELECT MAX(sayfasayisi) FROM kitap)
	
	21) En az sayfa sayısı olan kitabın sayfa sayısını listeleyiniz.
	
		SELECT MIN(kitap.sayfasayisi) AS maxsayfasayisi FROM kitap
	
	22) En az sayfası olan kitabın adını ve sayfa sayısını listeleyiniz.
	
		SELECT kitapadi, sayfasayisi FROM kitap WHERE sayfasayisi = (SELECT MIN(sayfasayisi) FROM kitap)
	
	23) Dram türündeki en fazla sayfası olan kitabın sayfa sayısını bulunuz.
	
		SELECT MAX(kitap.sayfasayisi) AS drammaxsayfasayisi FROM kitap JOIN tur ON tur.turadi = "Dram"
	
	24) numarası 15 olan öğrencinin okuduğu toplam sayfa sayısını bulunuz.
	
		SELECT ogrenci.ograd, SUM(kitap.sayfasayisi) AS okunansayfasayisi FROM ogrenci 
			JOIN islem ON islem.ogrno = ogrenci.ogrno  
			JOIN kitap ON kitap.kitapno = islem.kitapno
			WHERE ogrenci.ogrno = 15
			GROUP BY ogrenci.ograd	

	25) İsme göre öğrenci sayılarının adedini bulunuz.(Örn: ali 5 tane, ahmet 8 tane )

		SELECT ogrenci.ograd, COUNT(ogrenci.ogrno) AS ogrencisayisi FROM ogrenci GROUP BY ogrenci.ograd
	
	26) Her sınıftaki öğrenci sayısını bulunuz.
	
		SELECT ogrenci.sinif, COUNT(ogrenci.ogrno) AS ogrencisayisi FROM ogrenci GROUP BY ogrenci.sinif
	
	27) Her sınıftaki erkek ve kız öğrenci sayısını bulunuz.
	
		SELECT sinif, COUNT(CASE WHEN cinsiyet = "K" THEN ogrno END) AS kizogrencisayisi, COUNT(CASE WHEN cinsiyet = "E" THEN ogrno END) AS erkekogrencisayisi FROM ogrenci GROUP BY ogrenci.sinif
	
	28) Her öğrencinin adını, soyadını ve okuduğu toplam sayfa sayısını büyükten küçüğe doğru listeleyiniz.
	
		 SELECT ogrenci.ograd, ogrenci.ogrsoyad, SUM(kitap.sayfasayisi) AS toplamsayfasayisi FROM ogrenci 
			JOIN islem ON islem.ogrno = ogrenci.ogrno
			JOIN kitap ON kitap.kitapno = islem.kitapno
			GROUP BY ogrenci.ograd, ogrenci.ogrsoyad
	
	29) Her öğrencinin okuduğu kitap sayısını getiriniz.

		 SELECT ogrenci.ograd, ogrenci.ogrsoyad, COUNT(kitap.kitapno) AS toplamkitapsayisi FROM ogrenci 
			JOIN islem ON islem.ogrno = ogrenci.ogrno
			JOIN kitap ON kitap.kitapno = islem.kitapno
			GROUP BY ogrenci.ograd, ogrenci.ogrsoyad