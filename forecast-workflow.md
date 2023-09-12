# Forecast workflow


## Orchestration diagram

```mermaid
sequenceDiagram
	actor REV as Reviewer
	actor USA as Analyst
	participant ORC as Orchestration platform
        box rgb(60,40,40) Forecast Workflow Repo
        	participant WKM as Main branch
		participant WKF as Forecast branch
		participant WKA as Analyst forecast branch
	end
	participant USC as Forecast validator
        participant DWH as Data warehouse
	

	autonumber

		USA -->> ORC: Forecast round initializing
                Note over WKF: Life begins
		ORC -->> WKF: Forecast branch created
		Note over WKA: Life begins
		ORC -->> WKA: Analyst forecast branch created
		WKA -->> USA: Local environment created + configs
                DWH -->> USA: Input data retrieved
		critical Local forecast
			USA -->> USA: Data + model + model infrastructure
		end

		USA -->> WKA: Forecast submittecd: Analyst judgment as code pushed
                WKA -->> ORC: Notification
                DWH -->> ORC: Input data retrieved
                critical Forecast reproduced
                         ORC -->> ORC: Data + model + model infra + analyst judgment
                end
                ORC -->> USC: Request for mechanical forecast validation
                USC -->> ORC: Forecast validated
                ORC -->> WKA: Merge request created
	        ORC -->> WKF:  
                ORC -->> REV: Request for review and acceptance emailed
		REV -->> WKF: Forecast accepted, merge request executed
                REV -->> WKA:  
                Note over WKA: Life ends
		WKF -->> ORC: Forecast submission
                DWH -->> ORC: Input data retrieved
                critical Forecast reproduced
                         ORC -->> ORC: Data + model + model infra + analyst judgment
                end
		Note over ORC, DWH: Data submission
                ORC -->> DWH: Data stored in DWH
```

## Data-producing diagram

```mermaid
sequenceDiagram
	participant WRK as Forecast settings
	participant DWH as Data warehouse
	participant USR as User
	participant MOD as Model
	participant INF as Model infrastructure

	autonumber

    critical Local environment or orchestration platform
		WRK -->> DWH: Data request
		DWH -->> INF: Historical data + filtered data + foreign demand
		USR -->> INF: Forecast judgments
		MOD -->> INF: Model
		INF -->> DWH: Forecasted data
		Note over INF, DWH: Data submission only post-approval
    end
```


---

## Local environment dependencies

* Git (latest or any fairly recent)
* Matlab R2021b or later
* Python 3.11 with the `requests` package installed
* Access to the OGR-EIU repositories on GitHub - either SSH (more convenient) or PAT (easier)

---

## Set up the local environment manually

1. Create a `forecast` folder within which all dependencies will be cloned/installed next to each other. At the end of the setup process, the resulting structure should look like this:

```
local-forecast
|
|-- workflow-forecast
|
|-- model-cz
|
|-- model-infra
|
|-- toolset
|
|-- data-warehouse-client
|
|-- iris-toolbox
```

2. After creating the `local-forecast` folder, while being inside the `local-forecast` folder, clone the following OGR-EIU repositories


```
git clone https://github.com/OGR-EIU/workflow-forecast
git clone https://github.com/OGR-EIU/model-cz
git clone https://github.com/OGR-EIU/model-infra
git clone https://github.com/OGR-EIU/toolset
git clone https://github.com/OGR-EIU/data-warehouse-client
```

If you only have PAT based access, include your PAT as a string in the URL, e.g. 

```
git clone https://XXXXXXXXXXXXXXXXX@github.com/OGR-EIU/workflow-forecast
```

where `XXXXXXXXXXXXXXXXX` is your PAT. Mind the extra `@` inserted between the PAT and the rest of the URL.


3. Finally, clone the Iris Toolbox (public access, no SSH or PAT needed)

```
git clone https://github.com/IRIS-Solutins-Team/IRIS-Toolbox iris-toolbox
```


