# SQL Sorgu Alıştırmaları

Bu hafta SQL sorguları üzerine çalışıyorsunuz. Bugünkü alıştırmada sizin için hazırladığımız veritabanında aşağıda istediğimiz sonuçları elde etmenize yarayan SQL sorgularını oluşturacaksınız.

# Proje Kurulumu
Projeyi forklayın ve clonlayın. Tamamladığınızda da pushlayın.

## Kütüphane Bilgi Sistemi

Bu veritabanı, bir okulun kütüphanesinden öğrencilerin aldıkları kitapların bilgisini barındırmaktadır.

#Tablolar 
`ogrenci` tablosu öğrencilerin listesini tutar.
`islem` tablosu öğrencilerin kütüphaneden aldıkları kitapların bilgilerini tutar
`kitap` tablosu kütüphanedeki kitapların bilgisini tutar
`yazar` tablosu kitapların yazarları bilgisini tutar
`tur` tablosu kitapların türlerini tutar.

Tablo ilişiklerini görmek için [ktphn.png] dosyasına göz atın.

Yazdığınız sorguları buradan test edebilirsiniz: [https://ergineer.com/assets/materials/fkg36so5-kutuphanebilgisistemi-sql/]



# Görevler
Aşağıda istenilen sonuçlara ulaşabilmek için gerekli SQL sorgularını yazın. 



	1) ÖRNEK SORU: Yazar tablosunu KEMAL UYUMAZ isimli yazarı ekleyin.
			INSERT INTO yazar(yazarad,yazarsoyad) VALUES ("KEMAL" , "UYUMAZ")

	
	2) Biyografi türünü tür tablosuna ekleyiniz.
			INSERT INTO tur(turadi) VALUES ("Biyografi")
	
	3) 10A sınıfı olan ÇAĞLAR ÜZÜMCÜ isimli erkek, sınıfı 9B olan LEYLA ALAGÖZ isimli kız ve sınıfı 11C olan Ayşe Bektaş isimli kız öğrencileri tek sorguda ekleyin.
			INSERT INTO ogrenci(ograd,ogrsoyad,cinsiyet,sinif) 
				VALUES ("ÇAĞLAR" , "ÜZÜMCÜ","E",10A"),
					("LEYLA" , "ALAGÖZ","K",9B"),
					("Ayşe" , "Bektaş","K",11C");
			 
	
	
	4) Öğrenci tablosundaki rastgele bir öğrenciyi yazarlar tablosuna yazar olarak ekleyiniz.
			
			INSERT INTO yazar(yazarad,yazarsoyad)
			(SELECT ograd,ogrsoyad FROM ogrenci
				ORDER BY RAND()
				LIMIT 1;)
	
	
	5) Öğrenci numarası 10 ile 30 arasındaki öğrencileri yazar olarak ekleyiniz.
			INSERT INTO yazar(yazarad,yazarsoyad) 
				SELECT ograd,ogrsoyad FROM ogrenci 
					WHERE ogrno <30 AND ogrno>10;
	
	
	6) Nurettin Belek isimli yazarı ekleyip yazar numarasını yazdırınız.
	(Not: Otomatik arttırmada son arttırılan değer @@IDENTITY değişkeni içinde tutulur.)
			INSERT INTO yazar(yazarad,yazarsoyad) VALUES ("Nurettin" , "Belek")
				SELECT @IDENTITY as yazarno,
	
	
	7) 10B sınıfındaki öğrenci numarası 3 olan öğrenciyi 10C sınıfına geçirin.
			UPDATE ogrenci
			SET sinif='10C'
			WHERE ogrno="3";
	
	8) 9A sınıfındaki tüm öğrencileri 10A sınıfına aktarın
			UPDATE ogrenci
			SET sinif='10A'
			WHERE sinif="9A";
	
	
	9) Tüm öğrencilerin puanını 5 puan arttırın.
			UPDATE ogrenci
			SET puan=puan+5;
			
	
	
	10) 25 numaralı yazarı silin.
			DELETE FROM yazar
			WHERE yazarno =25;


	11) Doğum tarihi null olan öğrencileri listeleyin. (insert sorgusu ile girilen 3 öğrenci listelenecektir)
			SELECT* FROM ogrenci
			WHERE dtarih IS NULL
	
	
	12) Doğum tarihi null olan öğrencileri silin. 
			
			DELETE FROM ogrenci
			WHERE dtarih IS NULL
	
	13) Kitap tablosunda adı a ile başlayan kitapların puanlarını 2 artırın.
			UPDATE ogrenci
			SET puan=puan+5;
			WHERE kitapadi ILIKE 'a%'
	
	
	14) Kişisel Gelişim isimli bir tür oluşturun.
			INSERT INTO tur(turadi) VALUES ("Kişisel Gelişim")
	
	
	15) Kitap tablosundaki Başarı Rehberi kitabının türünü bu tür ile değiştirin.
			UPDATE kitap
			SET turadi=Kişisel Gelişim;
			WHERE kitapadi = "Başarı Rehberi"
	
	
	16) Öğrenci tablosunu kontrol etmek amaçlı tüm öğrencileri görüntüleyen "ogrencilistesi" adında bir prosedür oluşturun.
			CREATE OR REPLACE PROCEDURE ogrencilistesi() 
			LANGUAGE "sql"
			AS 
			$BODY$
				BEGIN
					SELECT * FROM ogrenci
				END; 
			$BODY$
			//CALL ogrencilistesi
			
	
	
	17) Öğrenci tablosuna yeni öğrenci eklemek için "ekle" adında bir prosedür oluşturun.
			CREATE OR REPLACE PROCEDURE ekle(IN ograd character varying, IN ogrsoyad character varying) 
			LANGUAGE "sql"as ogr
			AS 
			$BODY$
				BEGIN
					INSERT INTO ogrenci VALUES(ogr.ograd,o.ogrsoyad)
				END; 
			$BODY$
			//CALL ekle('emre','ert')
	
	18) Öğrenci noya göre öğrenci silebilmeyi sağlayan "sil" adında bir prosedür oluşturun.

			CREATE OR REPLACE PROCEDURE sil(IN ogrno integer) 
			LANGUAGE "sql"as ogr
			AS 
			$BODY$
				BEGIN
					DELETE FROM ogrenci WHERE ogrno=ogr.ogrno
				END; 
			$BODY$
			//CALL sil(1)
	
	
	19) Öğrenci numarasını kullanarak kolay bir biçimde öğrencinin sınıfını değiştirebileceğimiz bir prosedür oluşturun.

			CREATE OR REPLACE PROCEDURE change(IN sinif character varying,IN ogrno integer) 
			LANGUAGE "sql"as ogr
			AS 
			$BODY$
				BEGIN
					UPDATE ogrenci
						SET sinif=(ogr.sinif)
						WHERE ogrno=(ogr.ogrno);
				END; 
			$BODY$
			//CALL change('10A',2)
	
	
	20) Öğrenci adı ve soyadını "Ad Soyad" olarak birleştirip, ad soyada göre kolayca arama yapmayı sağlayan bir prosedür yazın.

			CREATE OR REPLACE PROCEDURE mergeandsearch(IN fullname character varying) 
			LANGUAGE "sql" 	as ogr
			AS 
			$BODY$
				BEGIN
					SELECT o.ograd,o.ogrsoyad FROM ogrenci as o
					   WHERE CONCAT(o.ograd,o.ogrsoyad)
						LIKE CONCAT('%ogr.fullname%')
				END; 
			$BODY$
			//CALL mergeadnsearch('10A',2)
	
	
	
	21) Daha önceden oluşturduğunu tüm prosedürleri silin.
			drop procedure if exists ekle,sil,change,mergeandsearch;
	
	
	#Esnek görevler (Esnek görevlerin hepsini Select in Select ile gerçekleştirmeniz beklenmektedir.)
	22) Select in select yöntemiyle dram türündeki kitapları listeleyiniz.
			SELECT * FROM kitap
				WHERE turno = 
				(SELECT turno FROM tur WHERE turadi = 'DRAM')
	
	
	23) Adı e harfi ile başlayan yazarların kitaplarını listeleyin.
			SELECT * FROM kitap
				WHERE yazarno IN 
				(SELECT yazarno FROM yazar WHERE yazarad LIKE "e%")
	
	24) Kitap okumayan öğrencileri listeleyiniz.
			SELECT * FROM ogrenci
				WHERE ogrno NOT IN 
				(SELECT ogrno FROM islem )
			
	
	
	25) Okunmayan kitapları listeleyiniz

	
	26) Mayıs ayında okunmayan kitapları listeleyiniz.
			SELECT * FROM kitap
				WHERE kitapno IN
				(SELECT kitapno FROM islem WHERE EXTRACT(MONTH from atarih::timestamp)!=5') AND EXTRACT(MONTH from vtarih::timestamp)!=5'
