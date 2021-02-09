# 4. Webscraping

Date: 2019-06-27

Architecture issue: [#252](https://github.com/home-assistant/architecture/issues/252)

## Status

Accepted

## Context

Webscraping is when we use code to mimic a user and log in to a website and get data in Home Assistant. This is usually needed because certain data sources/integrations do not offer an API.

Webscraping comes with the following downsides:

- Very fragile, break often. When the website is updated, the integration will need to be updated.
- Some vendors (like USPS) have IP banned users of such integrations
- Some rely on beautifulsoup (Python-based), others are relying on PhantomJS or other headless browsers, meaning we need to include a whole browser.

## Proposal

- We no longer accept any new integration that relies on webscraping
- We identify, deprecate for 2 releases and remove integrations that rely on webscraping
- It will still be possible to have custom integrations provide information via webscraping
- Generic integration to parse HTML are excluded from this decision

### Exception

An exception is made for the authentication phase. An integration is allowed to extract fields from forms. To make it more robust, data should not be gathered by scraping individual fields but instead scrape all fields at once.

## Consequences

Integrations that rely on webscraping will have to be maintained as custom integrations.
