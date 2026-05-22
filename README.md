# mrr-powerbi — Power BI consumer voor MRR-model

Deze repo is het **contract tussen Fabric (bron van waarheid) en Power BI (presentatie)**
voor het MRR-tracking project. Het bevat geen SQL-view definities — alleen
beschrijvingen van wat Power BI uit Fabric leest en hoe het wordt gebruikt.

## Inhoud

| Pad | Inhoud |
|-----|--------|
| `docs/Star_schema_PB.md` | Leidend document: 5 facts + 5 dims + relaties + measures |
| `docs/connection.md` | Hoe Power BI verbinden met het Fabric SQL-eindpunt |
| `powerbi/` | Power BI artifacts (.pbix, DAX measures, model-documentatie) |

## Architectuur-principes

- **Fabric = logica en waarheid** — MRR-berekeningen, status, MoM-vergelijking, signalen
- **Power BI = presentatie** — visualisaties, measures voor aggregatie, drill-through
- **Geen DAX-herbouw van Fabric-logica** — als een measure al op de fact staat,
  niet opnieuw berekenen in DAX (drift-risico met n8n en Excel)

Volledige principes staan in `docs/Star_schema_PB.md`.

## Hoe te beginnen

1. Lees `docs/Star_schema_PB.md` — dat beschrijft het volledige sterschema
2. Verbind Power BI met de Fabric Warehouse via stappen in `docs/connection.md`
3. Importeer de 5 dims + 5 facts als gewone tabellen
4. Leg relaties zoals beschreven in het sterschema-diagram
5. Bouw DAX-measures uit de measure-lijst in `Star_schema_PB.md` sectie 5
6. Commit Power BI artefacts naar `powerbi/`

## Updates aan view-definities

De SQL-view definities leven in de Fabric Warehouse. Deze repo bevat alleen
het contract — kolomnamen, types, grain, aggregatie-regels.

Wanneer view-definities veranderen (nieuwe maand, nieuw signaal, bugfix):
1. Wijziging vindt plaats in Fabric (deployed vanuit het bronsysteem)
2. `Star_schema_PB.md` wordt hier bijgewerkt als het contract wijzigt
3. Power BI semantisch model wordt aangepast als kolommen toegevoegd/verwijderd zijn

Niet-breaking wijzigingen (kolom toegevoegd, validatie verfijnd) hoeven geen
update aan deze repo. Breaking wijzigingen (kolom hernoemd, grain veranderd)
moeten hier eerst worden geagreerd voordat ze in Fabric gaan.
