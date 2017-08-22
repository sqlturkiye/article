
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

