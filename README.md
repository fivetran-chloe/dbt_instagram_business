[![Apache License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)
# Instagram Business

This package models Instagram Business data from [Fivetran's connector](https://fivetran.com/docs/applications/instagram-business). It uses data in the format described by [this ERD](https://fivetran.com/docs/applications/instagram-business#schemainformation).

The main focus of the package is to transform the core social media object tables into analytics-ready models that can be easily unioned in to other social media platform packages to get a single view. This is aided by our [Social Media Reporting package](https://github.com/fivetran/dbt_social_media_reporting).

## Models

This package contains transformation models, designed to work simultaneously with our [Instagram Business source package](https://github.com/fivetran/dbt_instagram_business_source) and our [multi-platform Social Media Reporting package](https://github.com/fivetran/dbt_social_media_reporting). A dependency on the source package is declared in this package's `packages.yml` file, so it will automatically download when you run `dbt deps`. The primary outputs of this package are described below.

| **model**                    | **description**                                                                                                        |
| ---------------------------- | ---------------------------------------------------------------------------------------------------------------------- |
| [instagram_business__posts](https://github.com/fivetran/dbt_instagram_business/blob/main/models/instagram_business__posts.sql)         | Each record represents the daily performance of a post or story. |

## Installation Instructions
Check [dbt Hub](https://hub.getdbt.com/) for the latest installation instructions, or [read the dbt docs](https://docs.getdbt.com/docs/package-management) for more information on installing packages.

Include in your `packages.yml`

```yaml
packages:
  - package: fivetran/instagram_business
    version: [">=0.1.0", "<0.2.0"]
```

## Package Maintenance
The Fivetran team maintaining this package **only** maintains the latest version. We recommend you keep your `packages.yml` updated with the [dbt hub latest version](https://hub.getdbt.com/fivetran/instagram_business/latest/). You may refer to the [CHANGELOG](/CHANGELOG.md) and release notes for more information on changes across versions.

## Configuration
By default, this package will look for your Instagram Business data in the `instagram_business` schema of your [target database](https://docs.getdbt.com/docs/running-a-dbt-project/using-the-command-line-interface/configure-your-profile). If this is not where your Instagram Business data is, add the following configuration to your `dbt_project.yml` file:

```yml
# dbt_project.yml

...
config-version: 2

vars:
    instagram_business_schema: your_schema_name
    instagram_business_database: your_database_name 
```

### Unioning Multiple Instagram Business Connectors
If you have multiple Instagram Business connectors in Fivetran and want to use this package on all of them simultaneously, we have provided functionality to do so. The package will union all of the data together and pass the unioned table(s) into the final models. You will be able to see which source it came from in the `source_relation` column(s) of each model. To use this functionality, you will need to set either (**note that you cannot use both**) the `union_schemas` or `union_databases` variables:

```yml
# dbt_project.yml
...
config-version: 2
vars:
    ##You may set EITHER the schemas variables below
    instagram_business_union_schemas: ['instagram_business_one','instagram_business_two']

    ##Or may set EITHER the databases variables below
    instagram_business_union_databases: ['instagram_business_one','instagram_business_two']
```
### Changing the Build Schema

By default, this package will build the Instagram Business staging models within a schema titled (`<target_schema>` + `_stg_instagram_business`) and the final Instagram Business models within a schema titled (`<target_schema>` + `_instagram_business`) in your target database. If this is not where you want your Instagram Business staging data to be written to, add the following configuration to your `dbt_project.yml` file:

```yml
# dbt_project.yml

...
models:
    instagram_business:
      +schema: my_new_schema_name # leave blank for just the target_schema
    instagram_business_source:
      +schema: my_new_schema_name # leave blank for just the target_schema
```

## Contributions

Don't see a model or specific metric you would like to be included? Notice any bugs when installing and running the package? If so, we highly encourage and welcome contributions to this package! 
Please create issues or open PRs against `main`. See [the Discourse post](https://discourse.getdbt.com/t/contributing-to-a-dbt-package/657) for information on how to contribute to a package.

## Database Support

This package has been tested on BigQuery, Snowflake, Redshift, Postgres, and Databricks.

### Databricks Dispatch Configuration
dbt `v0.20.0` introduced a new project-level dispatch configuration that enables an "override" setting for all dispatched macros. If you are using a Databricks destination with this package you will need to add the below (or a variation of the below) dispatch configuration within your `dbt_project.yml`. This is required in order for the package to accurately search for macros within the `dbt-labs/spark_utils` then the `dbt-labs/dbt_utils` packages respectively.
```yml
# dbt_project.yml

dispatch:
  - macro_namespace: dbt_utils
    search_order: ['spark_utils', 'dbt_utils']
```

## Resources:
- Provide [feedback](https://www.surveymonkey.com/r/DQ7K7WW) on our existing dbt packages or what you'd like to see next
- Have questions or feedback, or need help? Book a time during our office hours [here](https://calendly.com/fivetran-solutions-team/fivetran-solutions-team-office-hours) or email us at solutions@fivetran.com.
- Find all of Fivetran's pre-built dbt packages in our [dbt hub](https://hub.getdbt.com/fivetran/)
- Learn how to orchestrate dbt transformations with Fivetran [here](https://fivetran.com/docs/transformations/dbt).
- Learn more about Fivetran overall [in our docs](https://fivetran.com/docs)
- Check out [Fivetran's blog](https://fivetran.com/blog)
- Learn more about dbt [in the dbt docs](https://docs.getdbt.com/docs/introduction)
- Check out [Discourse](https://discourse.getdbt.com/) for commonly asked questions and answers
- Join the [chat](http://slack.getdbt.com/) on Slack for live discussions and support
- Find [dbt events](https://events.getdbt.com) near you
- Check out [the dbt blog](https://blog.getdbt.com/) for the latest news on dbt's development and best practices
