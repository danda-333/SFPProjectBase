# Utility V4

## Description

XML rozšíření SFP pro zjedonušení práce.

## Installation

Nahrajte všechny XML do SFP.
Nahrajte překlady do překladů.
Hotovo :)

V případě instalace na verzi nižší než 3, je třeba smazat dva formulare s pohledama které dostaly jiné identy.

Pokud bude problém s nahráváním souborů ConvertFromCSV.xml a SplitString.xml
Nahrajte pojednom nejdříve SplitString.xml a pak až ConvertFromCSV.xml

Pokud z nějakého důvodu budete nahrávat SplitString.xml znovu, tak to spadne. Pokud je to nutné nahrát musí se v db upravit procedura:

SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO

-- =============================================
-- Author:		Jan Němec
-- Create date: 04.01.2021
-- Description:	Vrati infomrace o objedktech v DB
-- =============================================
ALTER PROCEDURE [dbo].[ObjectInfoSelect]
AS
BEGIN
	-- SET NOCOUNT ON added to prevent extra result sets from
	-- interfering with SELECT statements.
	SET NOCOUNT ON;

    SELECT 
		o.name as Name, 
		o.[type] as [Type],
		s.name as [Schema]
	FROM 
		sys.objects as o INNER JOIN 
		sys.schemas as s ON o.schema_id = s.schema_id
	WHERE 
		[type] in ('P', 'V', 'FN', 'TF', 'IF')
	UNION
	SELECT 
		t.name as Name, 
		'UT' as [Type],
		s.name as [Schema]
	FROM 
		sys.types as t INNER JOIN
		sys.schemas as s ON t.schema_id = s.schema_id
	WHERE 
		t.is_table_type = 1 
END

## Changes

Původní v3 funkcionality zůstali.

Přidán hromadný import export.
  - do některých pohledů byli přidány tlačítka pro kopírování a export

Import probíhá přes formulář Importovač
  - Do textarea "CSV" nahrajete data k importu a tlačítkem zvolíte jak se má importovat
  - Formát musí být čárky[,]/středníky[;] jako oddělovače sloupců a nový řádky jako oddělovače řádků
  - Umí to semtam zpracovat i ohraničení uvozovkama["] a apostrofama['] nicmene ne moc dobře a bude to pravděpodobně psát chyby (na import těch pár věcí to nemá cenu zdokonalovat)
  - Pokud v pohledu použijete tlačítko pro kopírování -> je to správný formát k importu
  - Pokud chete importovat z exportu -> export -> export - TXT a zkopírujte všechno krom prvního řádku 
  - Import umí do jisté míry kontrolovat duplikáty 
    - pokud najde možný duplikát zahodí ho a pokračuje v importu

Přidán pohled pro poslední odeslané emaily
  - v pohledu se dá "rozkliknout" zpráva mailu na celou obrazovku (opětovným kliknutím zavřete)
