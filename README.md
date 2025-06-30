# ApocalypseLogs Dataset for KQL vs. the Zombie Apocalypse

This repository contains the datasets for the blog post ["KQL vs. the Zombie Apocalypse: Surviving with Data Queries"](https://rodtrent.substack.com/p/kql-vs-the-zombie-apocalypse-surviving). The datasets simulate a post-apocalyptic world with survivor camps, supply inventories, and zombie sightings. Use these files to follow along with the KQL queries in the blog post and practice analyzing data to survive the undead hordes.

## Dataset Overview

The dataset is provided as a ZIP file (`ApocalypseLogs_Dataset.zip`) containing three CSV files:

- **CampSupplies.csv**: Tracks food, water, and weapons for each survivor camp.
- **SurvivorCensus.csv**: Records population, infected count, and morale for each camp.
- **ZombieSightings.csv**: Logs zombie horde locations, sizes, and movement speeds.

## Prerequisites

To use these datasets with the blog post's KQL queries, you need:

- Access to a Kusto-compatible environment, such as [Azure Data Explorer (ADX)](https://dataexplorer.azure.com/).
- A free Azure account (sign up at [azure.com](https://azure.com)) to use the ADX free cluster.
- Basic familiarity with KQL (the blog post provides examples).
- A tool to unzip files (e.g., built-in utilities on Windows/Mac or `unzip` on Linux).
- (Optional) Azure CLI or PowerShell for advanced ingestion.

## Instructions to Ingest the Datasets

Follow these steps to download and ingest the datasets into Azure Data Explorer to run the KQL queries from the blog post.

### 1. Download the Dataset

1. Download the `ApocalypseLogs_Dataset.zip` file from this repository.
2. Unzip the file to a local directory. You should see three CSV files:
   - `CampSupplies.csv`
   - `SurvivorCensus.csv`
   - `ZombieSightings.csv`

### 2. Set Up Azure Data Explorer

1. Go to the [Azure Data Explorer web UI](https://dataexplorer.azure.com/).
2. Sign in with your Azure account or create a free account.
3. Use the free cluster (`help.cluster('help').database('Samples')`) or create your own cluster:
   - In the ADX portal, click "Add Cluster" and follow the prompts to create a new cluster.
   - Note the cluster URI (e.g., `https://yourcluster.region.kusto.windows.net`).
4. Create a new database:
   - In the Query pane, run:
     ```kql
     .create database ApocalypseLogs
     ```
   - Select the `ApocalypseLogs` database in the left sidebar.

### 3. Create Tables

Create tables in the `ApocalypseLogs` database to match the dataset schema. Run the following KQL commands in the ADX Query pane:

```kql
.create table CampSupplies (
    CampID: string,
    Timestamp: datetime,
    FoodUnits: int,
    WaterLiters: int,
    WeaponsCount: int,
    Latitude: real,
    Longitude: real
)

.create table SurvivorCensus (
    CampID: string,
    Timestamp: datetime,
    Population: int,
    InfectedCount: int,
    MoraleScore: int
)

.create table ZombieSightings (
    SightingID: string,
    Timestamp: datetime,
    Latitude: real,
    Longitude: real,
    HordeSize: int,
    MovementSpeed: real
)
```

### 4. Ingest the CSV Files

Ingest each CSV file into its corresponding table using the ADX web UI or KQL ingestion commands.

#### Option 1: Ingest via Web UI (Recommended for Beginners)

1. In the ADX web UI, select your `ApocalypseLogs` database in the left sidebar.
2. Right-click the `CampSupplies` table and select "Ingest data."
3. Choose "Upload file" and select `CampSupplies.csv` from your local directory.
4. In the ingestion wizard:
   - Ensure "CSV" is selected as the format.
   - Verify the schema mapping (columns should match the table schema).
   - Click "Next" and then "Start Ingestion."
5. Repeat for `SurvivorCensus.csv` and `ZombieSightings.csv`, ingesting into their respective tables.

#### Option 2: Ingest via KQL Commands

If you prefer using KQL, upload the CSV files to a publicly accessible blob storage (e.g., Azure Blob Storage) or a temporary URL, then run ingestion commands. Alternatively, use a local file path if your ADX cluster supports it. Example:

```kql
.ingest into table CampSupplies (
    h'https://your-storage.blob.core.windows.net/path/CampSupplies.csv'
) with (format='csv', ignoreFirstRecord=true)

.ingest into table SurvivorCensus (
    h'https://your-storage.blob.core.windows.net/path/SurvivorCensus.csv'
) with (format='csv', ignoreFirstRecord=true)

.ingest into table ZombieSightings (
    h'https://your-storage.blob.core.windows.net/path/ZombieSightings.csv'
) with (format='csv', ignoreFirstRecord=true)
```

Replace the URLs with the actual paths to your CSV files. If using local files, adjust the path accordingly (e.g., `file://C:/path/to/CampSupplies.csv`).

### 5. Verify the Data

Run a quick query to ensure the data loaded correctly:

```kql
CampSupplies | take 5
SurvivorCensus | take 5
ZombieSightings | take 5
```

You should see rows from each table, matching the CSV contents.

### 6. Follow Along with the Blog Post

1. Open the blog post ["KQL vs. the Zombie Apocalypse"](https://rodtrent.substack.com/p/kql-vs-the-zombie-apocalypse-surviving).
2. In the ADX Query pane, set the database to `ApocalypseLogs`.
3. Copy and paste the KQL queries from the blog post (e.g., finding low-supply camps, assessing infection risks, predicting zombie migrations).
4. Run each query and compare the results to the blog post’s examples.

## Troubleshooting

- **Ingestion Errors**: Ensure the CSV files are not modified and the column headers match the table schema. Check for encoding issues (UTF-8 is recommended).
- **Query Errors**: Verify the table names and column names match exactly. KQL is case-sensitive.
- **Access Issues**: Ensure you’re signed into the correct Azure account and have permissions for your ADX cluster.
- **Need Help?**: Check the [ADX documentation](https://docs.microsoft.com/en-us/azure/data-explorer/) or ask in the repository’s [Issues](https://github.com/your-username/your-repo/issues) section.

## Contributing

Found a bug or want to add more zombie data? Submit a pull request or open an issue! Let’s keep the apocalypse data-driven.

## License

This dataset is licensed under the MIT License. See [LICENSE](LICENSE) for details.
