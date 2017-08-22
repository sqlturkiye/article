# sp_rename ile Stored Procedure Adlarınızı Değiştirmeyin !

Saklı yordamlarımız yani Stored Procedure ‘lerimizin isimlerini sp_rename komutu ile değiştirmeyiniz. Bunun yerine DROP-CREATE
yapmanız en sağlıklı yöntemdir.

Çünkü sp_rename ile ismini değiştirdiğiniz Stored Procedure lerimizin sys.sql_modules ‘te ki definition alanında değişiklik gerçekleşmez
ve bu yüzden sp_helptext ile sp nin içeriğini görmek istediğimizde yanlış sonuç döndürür ve biz bu durumu hiç bir ortamımızda yaşamak
istemeyiz !

Örneğimizde ssp_Test_1 adında bir Stored Procedure oluşturuyoruz.

```
IF OBJECT_ID('dbo.ssp_Test_1') IS NOT NULL
	SET NOEXEC ON
GO
CREATE PROCEDURE dbo.ssp_Test_1
AS 
SELECT 'Test_1'
````

Oluşturduğumuz bu sp_ yi sys.sql_modules te sorguluyoruz.

``
SELECT
	*
FROM sys.sql_modules sm
``

![sp_rename_1](https://user-images.githubusercontent.com/31235407/29560637-50cf64a2-873b-11e7-8c71-b8da0bc21098.png)

Daha sonra sp_rename ile Stored Procedure ‘ümüzün adını değiştiriyoruz.

``
	EXEC sys.sp_rename @objname = N'ssp_Test_1'
					  ,@newname = N'ssp_Deneme_Change'
``

Tekrar sys.sql_modules te sorgulama yapıyoruz

``
SELECT
	*
FROM sys.sql_modules sm
``

![sp_rename_1](https://user-images.githubusercontent.com/31235407/29560637-50cf64a2-873b-11e7-8c71-b8da0bc21098.png)

Son olarak sp_helptext ile adını değiştirdiğimiz sp_ mizin içeriğini görmek istiyoruz.

exec sys.sp_helptext ssp_Deneme_Change

![sp_rename_2](https://user-images.githubusercontent.com/31235407/29560686-85199c8c-873b-11e7-99cd-e69c268f472e.png)

Gördüğünüz üzere ismini değiştirdiğimiz halde hem  sys.sql_modules altında herhangi bir değişiklik uygulanmadı hem de sp_helptext komutu ile çektiğimizde eski sp_ adı ile geldi.

 

Bu yüzden Procedure lerinizi bu şekilde rename etmek yerine mutlaka ilk önce kaldırıp sonra tekrar oluşturun. (DROP-CREATE)


