# Power BI - Fabric Warehouse verbinding

## Stappen in Power BI Desktop

    Get Data
      -> Microsoft Fabric
      -> Warehouse
      -> selecteer de Warehouse uit de workspace
      -> kies de gewenste views (facts + dims)
      -> bouw relaties + DAX-measures in het semantisch model

Alternatief: SQL Server-connector met de Warehouse SQL connection string.
Werkt identiek - handig als de Fabric-connector niet in de lijst staat.

## Te importeren views

Vijf facts:

- gld_subscription_month_status
- gld_subscription_month_invoice_bridge
- gld_subscription_month_rule_context
- gld_subscription_month_finance_context
- gld_subscription_month_decision

Vijf dims:

- dim_date
- dim_subscription
- dim_billing_contract
- dim_invoice
- dim_signaal

Alle objecten zitten in schema dbo van de Fabric Warehouse.

## Import vs. DirectQuery

Aanbeveling: Import met scheduled refresh (1x per dag).
Import: Power BI haalt data op en houdt in geheugen - visuals zijn snel.
DirectQuery: elke visual vuurt een SQL-query op Fabric - alleen bij grote datasets.

## Belangrijke aandachtspunten

lucanet_posting_count is een context-kolom, geen sommeerbare measure.
Aggregeer met MAX, MIN of COUNT DISTINCT - nooit met SUM.

Niet in DAX herbouwen: current_mrr, previous_mrr, is_significante_afwijking,
status-bepaling. Deze staan al op de fact in Fabric.

## Validatie

Een Power BI rapport toont dezelfde totale MRR als deze SQL-query:

    SELECT SUM(current_mrr)
    FROM dbo.gld_subscription_month_status
    WHERE mrr_month_key = <YYYYMM>;
