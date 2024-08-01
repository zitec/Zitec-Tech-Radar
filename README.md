# Zitec Technology Radar

This repo holds the data sources for Zitec's Technology Radars (adapted by us and based on the original [Thoughtworks Technology Radar](https://www.thoughtworks.com/radar)).

## Updating the Radars

All you have to do in order to update one of the radars is to find the corresponding file and edit it according to the [instructions](https://github.com/thoughtworks/build-your-own-radar?tab=readme-ov-file#setting-up-your-data).

Please make sure that the file is not broken by the edits. Although you can check the outcome using the steps in the publishing section below, please also validate the file locally before pushing it to the repo.

## Publishing the Radars

Data for each radar is pulled automatically from a public [Zitec GitHub repo](https://github.com/zitec/Zitec-Tech-Radar).

To publish a radar, just edit the files and push the changes to the [dedicated GitLab repository](https://gitlab.zitec.com/research/zitec-technology-radar). This private repo is then automatically synchronized with the public GitHub repo.

Finally, once the changes are synchornized (it can take up to 5 minutes) check the radar using one of these short links
* [Dev Tech Radar](https://zit.ec/techradar)
* [QA Tech Radar](https://zit.ec/qatechradar)

## Adding a new radar

To add a new radar:
* Create a new file in [CSV](https://github.com/thoughtworks/build-your-own-radar?tab=readme-ov-file#using-csv-data) or [JSON](https://github.com/thoughtworks/build-your-own-radar?tab=readme-ov-file#using-json-data) format using the linked insturction for the respective format.
* Push the file to the private GitLab repo and wait for it to be synchronized to GitHub.
* Go to the [Build Your Own Radar page](https://radar.thoughtworks.com/) and insert the URL of the raw file from GitHub (eg: https://raw.githubusercontent.com/zitec/Zitec-Tech-Radar/master/TechRadar.csv)
* Finally, build the radar, take the resulting URL and share it with any interested party.
* Optionally, you can generate a short URL (using the https://zit.ec domain) using https://rebrandly.com/. Please use the Tech Leads chat group to ask for support with this.

## [New] Tech Radar for Backstage platform

The tech radar is now available in JSON format, which can be used to display the radar in the Backstage platform. 

The current version of the radar unifies the data sources for the Dev and QA radars. The JSON file is available [here](./tech-radar.json).

The radar is validated in Gitlab CI to ensure that the JSON file is correctly formatted.

### Validating the JSON file on your local machine
To test the JSON file locally, follow these steps:
* [Install Genie](https://gitlab.zitec.com/research/genie-installer) on your machine (`bash <(curl -fsSL https://zit.ec/genie)`).
* Execute `gni install` if this is your first time running the project.
* Execute `gni run validate` to validate the JSON file.
