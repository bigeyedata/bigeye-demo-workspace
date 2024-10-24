# Overview
This repository serves as an example for managing Bigeye programmatically.  
It relies on the paradigm of having 2 workspaces in Bigeye–one for development and one for production.  
**Please note:** It is a working respository maintained by Solutions Engineering at Bigeye and updated as time permits.  


## Usage
All objects are versioned as yaml files and can be managed via Github workflows using the Bigeye CLI.  
The examples demonstrate the following:  
  
1. Deploying and deleting virtual tables  
2. Deploying and deleting cron schedules  
3. Deploying metric templates (deletes coming soon)  
4. Deploying deltas  
5. Utilizing the delta CICD command (Github only)  
6. Managing bigconfig as separate files  

### Configure authentication for workflows
Create a user with manage access to both workspaces, or create separate users for each workspace with manage access.  
Review the [documentation](https://docs.bigeye.com/docs/cli#configuration) for configuring access via the CLI.  
Once you have generated config.ini and credentials.ini for the user(s), you can store the contents of the files as secrets in Github.  
You'll notice in any workflow using the CLI, that the secrets are dumped to the necessary files and referenced in the Bigeye command.  


### Bigconfig
The [bigconfig](https://docs.bigeye.com/docs/bigconfig) deployment in this repository is separated into files that are easy to identify and understand what they contain.  
Rather than one sprawling bigconfig file, it is broken up and versioned as follows:  
* Saved metric definitions
    * [Metrics](https://docs.bigeye.com/docs/available-metrics) that are out of the box with Bigeye, separated by metric category
    * Metrics specifically used for FinOps use cases
* Tag defintions
    *  Files are named after the source, as it appears in Bigeye
    *  Columns that are common across all sources are in their own file
* Table deployments
    * Separated by the source name, as it appears in Bigeye
    * The file name is the name of the table in Bigeye
* Tag deployments
    * Separated by the source name, as it appears in Bigeye
    * The file name is the name of the collection in Bigeye

### Additional considerations
When using virtual tables:  
* When applicable, create a virtual table where you can deploy multiple metrics. Bigeye can batch metrics to reduce the number of queries against a source.
* **Implement a naming convention** for tables and/or columns whenever possible to take advantage of the auto apply on indexing [functionality](https://docs.bigeye.com/docs/bigconfig#auto-apply-on-indexing) of bigconfig. 

This functionality can also be leveraged if you have a naming convention in place for your standard tables/columns across sources.  
Even if there is enough similarity, you can utilize the regex feature of tag definitions to reduce the amount of code needed for deployments. See the additional example in the Bigeye [docs](https://docs.bigeye.com/docs/bigconfig#tag-definitions-optional)
