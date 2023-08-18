
# Big picture of forecast workflow

```mermaid
sequenceDiagram
	participant SRC as Data source
	participant PUMP as Data pump
	participant TRAN as Post-pump transformer
	participant DWH as Data warehouse
    participant PREM as Pre-model transformer
    participant FILR as Filtering report
    participant GLOB as Global model
    participant CTRY as Country model
    participant AGGR as Aggregator of FD
    participant FORR as Forecast report
    participant CSVF as Export CSV

    loop Daily, nondaily and manual pumps
        SRC -->> PUMP: Data acquisition {CNB, ECB, FRED, manual}
        PUMP -->> DWH: Data submition into DWH
        DWH -->> TRAN: Post-pump data transformation
        TRAN -->> DWH: Data submition into DWH
    end

    loop Individual countries
        DWH -->> PREM: Historical data
        PREM -->> DWH: Trend-cycle filtering
    end

    DWH ->> FILR: Historical data + Trend-cycle filtering {US, EA, CZ}

    DWH -->> GLOB: Historical data + Trend-cycle data + Previous foreign demand {US, EA}
    GLOB -->> DWH: Global forecast {W0, US, EA}

    DWH -->> AGGR: Global forecast {US, EA}
    AGGR -->> DWH: Country-specific foreign demand {US, EA, CZ}

    loop Individual countries
        DWH -->> CTRY: Historical data + Trend-cycle data + Foreign demand
        CTRY -->> DWH: Country forecast
    end

	DWH -->> FORR: Forecast report

	DWH -->> CSVF: Export CSV
```
