# Open Data

> People should be able to collaborate on Open Data the same way we collaborate on Open Source code.

## Motivation
[Open data is a public good](https://en.wikipedia.org/wiki/Open_data#Open_Data_as_commons). As such, it is an area where individual [[incentives]] collide with collective ones. For example, as an organization, [spending time curating and maintaining datasets for other companies to use doesn't make sense](https://en.wikipedia.org/wiki/Economics_of_open_data) unless that's how you make money.

On the other hand, we've seen what [Open Data can do for us](https://twitter.com/patrickc/status/1256987283141492736). Used properly, data is a great tool to educate and [[Coordination | coordinate]] people. Data helps, well..., data-driven decision making! 

**We need better tools, protocols, and mechanisms to improve the Open Data ecosystem**.

Organizations have been doing BI for a while but that knowledge hasn't jumped to the Open Data movement.

## Ecosystem Principles
- **Easy**. Tools should make easy to create, curate and share datasets.
- **Versioned and Modular**. The main abstractions (things like `dataset`, `relation`) could be updated, forked and discussed as code in version controlled repositories.
	- E.g: fork `ourworldindata.usa_covid_cases`, improve it and publish it. People now can `select * from youruser.usa_covid_cases`.
	- Modeling could be limited to SQL and done with something like `dbt` so everything comes down to RAW data and the SQL `dbt` code.
		- This provided a declarative way of defining the datasets schema and other properties like _relations_ or _tests_.
- **Reproducible and Verifiable**. People should be able to trust the final datasets without having to recompute them from scratch. [Software defined assets](https://dagster.io/blog/software-defined-assets).
- **Permissionless**. Anyone should be able to add/update/fix datasets and relations between them. GitHub style collaboration. Upload CSV/Parquet or point to a remote one and start exploring!
- **Aligned Incentives**. Curators should have incentives to improve the datasets. Data is messy after all, but a good set of incentives could make great datasets surface and reward contributors accordingly.
	- Curating the data provides compounding benefits for the entire community!
	- Surfacing and creating great datasets should be rewarded.
- **Open Source**. Datasets could be stored in a decentralized way using something like IPFS and queried via tools like [DuckDB WASM Shell](https://shell.duckdb.org/).
	- Integrates with open source ETL tools like Singer/Airbyte.

## Components
### Packaging
- **Distribution**. Decentralized way. Could work in a closed network too!
- **Versioning**. Should be able to manage diffs and incremental changes in a smart way. E.g: only storing the new rows or columns.
- **Permanence**. Each version should be accessible and permanent.
- **Indexing**. Should be easy to list datasets matching a certain pattern or reading from a certain source. Datasets could be linked to a `Datafile` with description, visualizations, ...
- **Formatting**. Allow people to access the data in their preferred format (CSV, Parquet, ...)
- **Social**. Stars, users, citations, attaching default visualizations (d3, [Vega](https://vega.github.io/), [Vegafusion](https://github.com/vegafusion/vegafusion/), and others), ...
	- Importing datasets. Making possible to `data fork user/data`, improve something and publish the resulting dataset back.

### Storage
- Use smart protocol for storing the data so rows/columns are not duplicated and new ones can be built on top of others. 
- Compare local hash with remote hash to know if anything needs to be updated
- Inmutable(append only) source datasets?
- Centralized (S3, GCS, ...) and Decentralized (IPFS, Hypercore, ...).
- Support many types of data. Tables, Geospatial, Images, ...

### Transformations
- Packaged Lambda transformations (WASM/Docker). 
	- For tabular data, starting with just SQL might be great. 
	- Pyodite + DuckDB for transformations could cover a large area.
- Defined as code. E.g: YAML files with the source datasets and the transformations. Similar to how Pachyderm/Kamu/Holium are doing these.
- Can be run locally and remotely.
- Open transformations could empower a bunch of use cases:
	- Detect outliers automatically.
	- Detect suspicions records like a categorical variable value that only appears one time while others values appear many times.
	- Enrich data smartly (matcher + augmenter). If it detects a date, add the day of wee. If it detects latitude and longitude, adds country/city.

### Visualizations
- Suggest basic charts (bars, lines, time series, clustering).
- Smart drill downs.

## Current Landscape
Fixing Open Data is something people have been working on for a while. These are some of the solutions I'm aware of, but I'm sure there are much more tools and approaches out there.

- **Classical Open Data portals**. They usually provide static datasets with different degrees of curation, freshness, and, formats.[Google Dataset Search](https://datasetsearch.research.google.com) surfaces a lot of them.  This works well as a way of sharing single datasets but makes it very hard to curate and connect them openly given the lack of a standard.
- **Decentralized Datasets**. There are also [IPFS datasets](https://awesome.ipfs.io/datasets/). Similar to the classical approach, but decentralized on IPFS. [Datasets are being added continuously](https://youtu.be/-9rKtrwMkG0?t=638). The main challenge of this approach is discoverability.
	- [There is also `datadex` by Juan Benet](https://juan.benet.ai/blog/2014-03-11-discussion-scienceexchange/) (IPFS Creator). It shares some of the  [ideas](https://github.com/jbenet/data/blob/master/dev/designdoc.md) outlined in this document.
	- [Qri](https://qri.io/). An evolution of the classical open portals that added [[decentralization]] (IPFS) and computing on top of the data. Sadly, [it came to an end early in 2022](https://qri.io/winding_down). It's the closest thing to the ideal I shared earlier I'm aware of.
	- [Holium](https://docs.holium.org/). An open source protocol dedicated to the management of data connected through transformations. Similar to Pachyderm but using WASM and IPFS.
	- [Dolt](https://docs.dolthub.com/) is another interesting project in the space with some awesome data structures. They also [do data bounties](https://www.dolthub.com/repositories/dolthub/us-businesses)!
	- [Trino](https://trino.io/) is a distributed query engine for data. It could work on top of IPFS if it supported it.
- In the web3 space, [Ocean Protocol](https://oceanprotocol.com/) and [The Graph](https://thegraph.com/) have designed a great incentive landscape and provided tools to share and discover data. For now, I think they only work on blockchain related datasets.
	- Some web3 organizations are [thinking about data](https://docs.indexcoop.com/our-products/data-economy-index-data), [[Incentives]] and [[Governance]].
- There are also some interesting databases in the space ([DuckDB](https://duckdb.org/)) that focus on decentralizing the querying capabilities, using technologies like WASM.
	- This makes possible an intermediary step in which you could read Parquet files from IPFS, model the data with `dbt` and write them back on IPFS.

## Extra Thoughts
- There are already open source projects like [Airbyte](https://airbyte.com/) that could be used to build open data connectors. It would make possible replicating something from a random source (like the Ethereum blockchain) to a destination (like IPFS).
- With a common standard for the metadata, datasets could be indexed with a computation framework on top of IPFS.
- Querying could also be archived with such computation framework. There are also some databases ([Ceramic](https://ceramic.network/), [Crust](https://www.crust.network/), [Textile Threads](https://github.com/textileio/go-threads)) that work on IPFS but they don't support this use case.
- [Making a SQL interface](https://twitter.com/josephjacks_/status/1492931290416365568) to query and mix these datasets could be a great step forward since it'll enable tooling like `dbt` to be used on top of it. **Data-as-code**.
	- SQL should be enough for unlocking most part of the potential. E.g: joining Wikipedia data to Our World In Data.
	- There are some [web3 DAOs already using `dbt` to improve data models](https://github.com/MetricsDAO/harmony_dbt/tree/main/models/metrics)!

### Related Projects
- [Kamu](https://www.kamu.dev/)
- [Dolt](https://github.com/dolthub/dolt)
- [Qri](https://qri.io/)
- [Holium](https://docs.holium.org/)
- [dbhub](https://dbhub.io/)
- [Quilt](https://github.com/quiltdata/quilt)
- [DVC](https://github.com/iterative/dvc)
- [Minerva](https://github.com/bdchain/Minerva)
- [Ocean Protocol](https://oceanprotocol.com/technology/compute-to-data)
- [Akash](https://akash.network/)
- [Fission](https://fission.codes/)
- [Kylin](https://wiki.kylin.network/getting-started/project-details/project-architecture/data-analytics).
- [IPFS Compute](https://github.com/adlrocha/ipfs-compute).