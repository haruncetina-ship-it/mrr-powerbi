# Power BI artefacts

Deze map is bestemd voor Power BI semantisch model en rapport-artefacten:

- .pbix bestanden (semantisch model + rapport)
- .bim of model.json (Tabular model definities indien gebruikt)
- DAX measure-definities als losse bestanden (optioneel, voor versiebeheer)
- Screenshots van rapport-pagina's voor referentie

## Bestandsnaam-conventie

- mrr_overzicht.pbix      -- hoofd-rapport met 3 lagen drill-through
- mrr_signalen.pbix       -- afwijking-detectie en decision-tracking
- mrr_bc_overzicht.pbix   -- apart BC-niveau rapport (Laag 0)

## .gitignore

.pbix bestanden zijn binary. Voor grotere bestanden overweeg Git LFS,
of houd alleen de meest recente productie-versie in de repo en gebruik
Fabric workspace voor de ontwikkel-iteraties.
