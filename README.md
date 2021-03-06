## Deployment

This repository maintains multiple bots:

* `open_gazettes_fr_bodacc`: daily scraper of the current year
* `open_gazettes_fr_bodacc_2012`: one-off scraper of 2012
* `open_gazettes_fr_bodacc_2013`: one-off scraper of 2013
* `open_gazettes_fr_bodacc_2014`: one-off scraper of 2014
* `open_gazettes_fr_bodacc_2015`: one-off scraper of 2015

To modify a bot, first run:

    bundle exec rake configure year=2012

Or for the current year:

    bundle exec rake configure

The run `turbot` commands as usual.

## Development

The `data` directory is structured to match the output of:

    wget -r -nc -P data ftp://echanges.dila.gouv.fr/BODACC/

If the documentation is downloaded by `wget`, you can extract the XSD:

    rake xsd

And generate SVG from XSD (requires Java):

    rake svg

You can open the SVG files in your browser.

### Primary data

Run the scraper:

    ruby scraper.rb

Or:

    turbot bots:dump

Run the commands with `MORPH_URL=1` to run in production mode, which disable caching and filtering with environment variables.

Change the log level:

    TURBOT_LEVEL=DEBUG ruby scraper.rb

See log messages only:

    TURBOT_QUIET=1 ruby scraper.rb

Run a specific year:

    year=2015 ruby scraper.rb

Start at a specific issue number:

    from_issue_number=20150100 ruby scraper.rb

Stop at a specific issue number:

    to_issue_number=20150100 ruby scraper.rb

Run a specific format:

    format=RCS-A ruby scraper.rb

### Transformed data

Run the transformer:

    cat scraper.out | ruby transformer.rb

## Documentation

* [BODACC editions](http://www.bodacc.fr/Bodacc/Mieux-connaitre-le-Bodacc#Avis)

Once you have a company number, you can compose the URL to a company page like https://www.infogreffe.fr/societes/entreprise-societe/303514160

## XML Schema

On 2013-05-24, `RCI_V10.xsd` was updated to change the `maxLength` of `NomDenomination_Type` from `200` to `1000`, but the version number of the XSD file was not changed.

From 2014-04-01, `Bilan_V06.xsd` adds an optional `descriptif` field under `depot`, and `PCL_V12.xsd` adds values to the `nature` code list under `jugement`.

From 2015-02-16, `PCL_V13.xsd` adds a required `denominationEIRL` field under `personnePhysique`.
