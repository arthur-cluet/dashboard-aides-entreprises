---
title : CONFIGURATION - SOURCES/DATASETS
classes: wide
categories:
  - configfiles
tags:
  - tutorial
  - configuration
  - data
  - global
  - schema
toc: false
toc_label: " contents"
toc_sticky: true
---

--------

## Config files

[Back to config files list]({{site.baseurl}}/configuration/config-configs)

## File location

```shell
frontend
│   README.md
│   .env
│   nuxt.config.js
│
└─── configs
    │
    └─── dev
        │
        └─── appConfigData.js

```

## The DATA configuration file

The `appConfigData.js` file manages the data you will display in your instance.

{% include figure image_path="/static/schemas/DASHBOARD_WIREFRAME-data-01.png" alt="" %}

This `.js` file can be changed in development mode, but it will usually be transformed into a `.json` file. The later will be stored in `frontend/static/configs/`


### Global parameters

```json
{
  "help" : help string for developpers ,
  "dataSource" : for API references to load data sources **- in development**  ,
  "filters" : **list** **- in development** ,
  "defaultDataSetup" : data sources loaded for every route / described inn the next part ,
  "routesData" : data sources loaded for a specific route / described in the next paragraph ,
}

```

### The `defaultDataSetup` | `routesData` field structure :

The following json describes how a particular route indicating it needs to load the dataset with 
`ìd`=`national-aides-raw`
will be loaded and stored in vuex store.

```json
{
  "routesData" : {
    "help": 'data sources not loaded at init but depending on routes',
    "initData": {
      "help": '',
      "store": [
          ### **list** : load a source and store it to store/data/initData
          {
            "id": 'national-aides-raw',
            "dataset": 'fds',
            "help": 'serie chiffres aides à la maille nationale',
            "from": 'static',
            "url": `${DATASETS_REPO_BASE}/aides/aides-maille-national-minify.json`,
            "backupUrl": `${DATASETS_FOLDER}/prod/aides/aides-maille-national.json`,
            "displayed": true,
            "copyTo": [
              ### **list** : map and copy values to a store/data/SpecialStore key
              {
                "fieldToCopy": undefined,
                "from": { objectRef: 0 },
                "help": 'copy to another dataset (id) in displayedData | initData',
                "toSpecialStore": 'focusObject',
                "format": undefined
              },
              {
                "fieldToCopy": 'montant',
                "from": { objectRef: 0 },
                "help": 'copy to another dataset (id) in displayedData | initData',
                "toSpecialStore": 'montant',
                "format": [
                  {
                    "utilsFnName": 'toMillionsOrElse',
                    "params": { divider: 1000000, fixed: 2 }
                  }
                ]
              }
            ]
          },
      ]
   }
}

```
