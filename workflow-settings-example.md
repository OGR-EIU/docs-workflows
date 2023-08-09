```
{
    "dependencies": [
        {
            "url": "",
            "commit_id": "",
            "reference": ""
        }
    ],
    "step_settings": [
        {
            "step_id": "step_1",
            "start_date": "2020-01-01",
            "end_date": "2023-08-08",
            "request_mapping": {
                "FRED.US.Q.S0LEV.RGDP": "GDPC1",
                "FRED.US.Q.S0LEV.NGDP": "GDP",
                "FRED.US.Q.SULEV.BOP_CA": "IEABCN",
                "FRED.US.M.SULEV.NIR_ST": "AMBOR3M"
            },
            "data_request": [
                {
                    "concept": "RGDP",
                    "geography": "US",
                    "data_source": "FRED",
                    "transformation": "S0LEV",
                    "frequency": "Q"
                },
                {
                    "concept": "NGDP",
                    "geography": "US",
                    "data_source": "FRED",
                    "transformation": "S0LEV",
                    "frequency": "Q"
                },
                {
                    "concept": "BOP_CA",
                    "geography": "US",
                    "data_source": "FRED",
                    "transformation": "SULEV",
                    "frequency": "Q"
                },
                {
                    "concept": "NIR_ST",
                    "geography": "US",
                    "data_source": "FRED",
                    "transformation": "SULEV",
                    "frequency": "M"
                }
            ]
        },
        {
            "step_id": "step_2",
            "data_request": [
                {
                    "concept": "RGDP",
                    "geography": "US",
                    "data_source": "FRED",
                    "transformation": "S0LEV",
                    "frequency": "Q"
                },
                {
                    "concept": "NGDP",
                    "geography": "US",
                    "data_source": "FRED",
                    "transformation": "S0LEV",
                    "frequency": "Q"
                },
                {
                    "concept": "BOP_CA",
                    "geography": "US",
                    "data_source": "FRED",
                    "transformation": "SULEV",
                    "frequency": "Q"
                },
                {
                    "concept": "NIR_ST",
                    "geography": "US",
                    "data_source": "FRED",
                    "transformation": "SULEV",
                    "frequency": "M"
                }
            ]
        }
    ]
}
```
