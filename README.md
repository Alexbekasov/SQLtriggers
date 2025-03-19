# SQLtriggers


Create TRIGGER liinaKustutamine
ON linnad --tabelinimi, mis on vaja jälgida
FOR DELETE
AS
INSERT INTO logi(kasutaja, aeg, toiming, andmed)
SELECT
SUSER_NAME(), --USER
GETDATE(),
'on tehtud DELETE käsk',
CONCAT ('linn: ',deleted.linnanimi, ', elanike arv:', deleted.rahvaarv)
FROM deleted;

--kontroll
DELETE FROM linnad WHERE linnID=1;
select * from linnad;
select * from logi;


USE [trigerTARgv24]
GO
/****** Object:  Trigger [dbo].[liinaKustutamine]    Script Date: 19.03.2025 10:48:29 ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
ALTER TRIGGER [dbo].[liinaKustutamine]
ON [dbo].[linnad] --tabelinimi, mis on vaja jälgida
FOR DELETE
AS
INSERT INTO logi(kasutaja, aeg, toiming, andmed)
SELECT
SUSER_NAME(), --USER
GETDATE(),
'on tehtud DELETE käsk',
CONCAT ('linn: ',deleted.linnanimi, ', elanike arv:', deleted.rahvaarv)
FROM deleted;

--------------------------------------------------------------------------

Create TRIGGER liinaUuendamine
ON linnad --tabelinimi, mis on vaja jälgida
FOR DELETE
AS
INSERT INTO logi(kasutaja, aeg, toiming, andmed)
SELECT
SUSER_NAME(), --USER
GETDATE(),
'on tehtud DELETE käsk',
CONCAT ('vanad andmed -linn: ', deleted.linnanimi,
', elanike arv:', deleted.rahvaarv,
'\n uued andmed -linn: ', inserted.linnanimi,
', elanike arv:', inserted.rahvaarv)
FROM deleted
INNER JOIN inserted
ON deleted.linnID=inserted.linnID;
--kontroll
UPDATE linnad SET rahvaarv=2500
WHERE linnID=3;
select * from linnad;
select * from logi;



USE [trigerTARgv24]
GO
/****** Object:  Trigger [dbo].[liinaUuendamine]    Script Date: 19.03.2025 10:48:07 ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
ALTER TRIGGER [dbo].[liinaUuendamine]
ON [dbo].[linnad] --tabelinimi, mis on vaja jälgida
FOR DELETE
AS
INSERT INTO logi(kasutaja, aeg, toiming, andmed)
SELECT
SUSER_NAME(), --USER
GETDATE(),
'on tehtud DELETE käsk',
CONCAT ('vanad andmed -linn: ', deleted.linnanimi,
', elanike arv:', deleted.rahvaarv,
'\n uued andmed -linn: ', inserted.linnanimi,
', elanike arv:', inserted.rahvaarv)
FROM deleted
INNER JOIN inserted
ON deleted.linnID=inserted.linnID;

--------------------------------------------------------------------------
  

USE [trigerTARgv24]
GO
/****** Object:  Trigger [dbo].[linnaLisamine]    Script Date: 19.03.2025 10:48:55 ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
ALTER TRIGGER [dbo].[linnaLisamine]
ON [dbo].[linnad] --tabelinimi, mis on vaja jälgida
FOR INSERT
AS
INSERT INTO logi(kasutaja, aeg, toiming, andmed)
SELECT
SUSER_NAME(), --USER
GETDATE(),
'on tehtud INSERT käsk',
CONCAT (inserted.linnanimi, ', elanike arv:', inserted.rahvaarv)
FROM inserted;

--------------------------------------------------------------------------

XAMPP

INSERT INTO logi(kasutaja, aeg, toiming, andmed)
values(
USER(), 
NOW(),
'on tehtud INSERT käsk',
CONCAT ('linn: ', new.linnanimi, ', elanike arv:', new.rahvaarv)
)



INSERT INTO logi(kasutaja, aeg, toiming, andmed)
SELECT
USER(),
NOW(),
'on tehtud DELETE käsk',
CONCAT ('vanad andmed -linn: ', OLD.linnanimi,
', elanike arv:', OLD.rahvaarv,
'\n uued andmed -linn: ', NEW.linnanimi,
', elanike arv:', NEW.rahvaarv)
FROM linnad l
INNER JOIN linnad li
ON l.linnID=li.linnID
WHERE NEW.linn.ID=l.linnID


