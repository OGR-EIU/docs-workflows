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
    participant SNPR as Snapshot report
    participant SNPF as Export snapshot

    loop Daily, nondaily and manual pumps
        SRC -->> PUMP: Data acquisition {CNB, ECB, FRED, manual}
        PUMP -->> DWH: Acquired data
        DWH -->> TRAN: Post-pump data transformation
        TRAN -->> DWH: Transformed data
    end

    loop Individual countries
        DWH -->> PREM: Historical data
        PREM -->> DWH: Trend-cycle filtering
        DWH ->> FILR: Historical data + Trend-cycle filtering {US, EA, CZ}
    end

    DWH -->> GLOB: Historical data + Trend-cycle data + Previous foreign demand {US, EA}
    GLOB -->> DWH: Global forecast {W0, US, EA}

    DWH -->> AGGR: Global forecast {US, EA}
    AGGR -->> DWH: Country-specific foreign demand {US, EA, CZ}

    loop Individual countries
        DWH -->> CTRY: Historical data + Trend-cycle data + Foreign demand
        CTRY -->> DWH: Country forecast
        DWH -->> FORR: Country forecast
    end

	DWH -->> SNPR: Data snapshot
	DWH -->> SNPF: Data snapshot
```
