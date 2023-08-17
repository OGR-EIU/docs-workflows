
# Big picture of forecast workflow

```mermaid
sequenceDiagram
    participant PREM as Pre-model transformer
    participant REPF as Filtering report
    participant DWH as Data warehouse
    participant GLOB as Global model
    participant CTRY as Country model
    participant AGGR as Aggregator of FD

    loop Individual countries
        DWH -->> PREM: Historical data
        PREM -->> DWH: Trend-cycle filtering
    end

    DWH ->> REPF: Historical data + Trend-cycle filtering {US, EA, CZ}

    DWH -->> GLOB: Historical data + Trend-cycle data + Previous foreign demand {US, EA}
    GLOB -->> DWH: Global forecast {W0, US, EA}

    DWH -->> AGGR: Global forecast {US, EA}
    AGGR -->> DWH: Country-specific foreign demand {US, EA, CZ}

    loop Individual countries
        DWH -->> CTRY: Historical data + Trend-cycle data + Foreign demand
        CTRY -->> DWH: Country forecast
    end
```
