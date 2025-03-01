---
pcx_content_type: concept
title: Changelog

---

# Changelog

## Purpose

The purpose of a changelog is to log or record notable changes.

## Tone

instructional, straightforward

## content_type

`changelog`

## Ownership

Developers and engineers maintain changelogs manually or via an automated process that their team owns. PCX provides a review but should not own creating or writing changelogs.

The structure of the page can differ depending on the frequency or type of page.

{{<Aside type="note">}}
Do not use the following terms: change log (two words), release notes, what's new, or README.

"What's New" is a specific [content type](https://www.cloudflare.com/whats-new/) for marketing communication.
{{</Aside>}}

## Structure

When creating a changelog, you need a Markdown page file and a corresponding YAML file in the [`/data/changelogs` folder](https://github.com/cloudflare/cloudflare-docs/tree/production/data/changelogs).

The combination of these files allows us to:

- Render traditional changelog content on an [HTML page](/stream/changelog/).
- Programmatically create an [RSS feed](/stream/changelog/index.xml) with the changelog content.
- Pull all our changelog content into a [Cloudflare-wide changelog](/changelog/).

### Markdown file

Your Markdown file needs to have several special values to pull in the changelog information. These values are highlighted in the sample page.

```
---
header: /queues/changelog.md
highlight: [5-9, 14]
---

---
pcx_content_type: changelog
title: Changelog
weight: 11
layout: changelog
changelog_file_name: <YAML_FILE_NAME> (for example, queues)
outputs:
    - html
    - rss
---

# Changelog

{{</*product-changelog*/>}}
```

### YAML file

The `product-changelog` component renders data that lives in a file within the [`/data/changelogs` folder](https://github.com/cloudflare/cloudflare-docs/tree/production/data/changelogs).

{{<definitions>}}

- `link` {{<type>}}string{{</type>}} {{<prop-meta>}}required{{</prop-meta>}}

  - Relative path to the changelog page, such as `"/queues/changelog/"`.

- `productName` {{<type>}}string{{</type>}} {{<prop-meta>}}required{{</prop-meta>}}

  - Product name to display on the [changelog](/changelog/) product filter list, as well as other areas of the template.

- `entries` {{<type>}}object{{</type>}} {{<prop-meta>}}required{{</prop-meta>}}
    - `publish_date` {{<type>}}string{{</type>}} {{<prop-meta>}}required{{</prop-meta>}}

        - Date of published change, formatted as `YYYY-MM-DD`.

     - `title` {{<type>}}string{{</type>}} {{<prop-meta>}}optional{{</prop-meta>}}

        - Name of published change. Optional, but highly encouraged.
    
     - `description` {{<type>}}string{{</type>}} {{<prop-meta>}}required{{</prop-meta>}}

        - Markdown string that also follows YAML conventions. For multi-line strings, start the entry with `|-` and then type on an indented new line.

{{</definitions>}}

```yml
---
header: /data/changelogs/queues.yaml
---
---
link: "/queues/changelog/"
productName: Queues
entries:
- publish_date: '2023-03-28'
  title: Consumer concurrency (enabled)
  description: Queue consumers will now [automatically scale up](/queues/learning/consumer-concurrency/)
    based on the number of messages being written to the queue. To control or limit
    concurrency, you can explicitly define a [`max_concurrency`](/queues/platform/configuration/#consumer)
    for your consumer.
- publish_date: '2023-03-15'
  title: Consumer concurrency (upcoming)
  description: |-
    Queue consumers will soon automatically scale up concurrently as a queues' backlog grows in order to keep overall message processing latency down. Concurrency will be enabled on all existing queues by 2023-03-28.

    **To opt-out, or to configure a fixed maximum concurrency**, set `max_concurrency = 1` in your `wrangler.toml` file or via [the queues dashboard](https://dash.cloudflare.com/?to=/:account/queues).

    **To opt-in, you do not need to take any action**: your consumer will begin to scale out as needed to keep up with your message backlog. It will scale back down as the backlog shrinks, and/or if a consumer starts to generate a higher rate of errors. To learn more about how consumers scale, refer to the [consumer concurrency](/queues/learning/consumer-concurrency/) documentation.
- publish_date: '2023-03-02'
  title: Explicit acknowledgement (new feature)
  description: |-
    You can now [acknowledge individual messages with a batch](/queues/learning/batching-retries/#explicit-acknowledgement) by calling `.ack()` on a message.

    This allows you to mark a message as delivered as you process it within a batch, and avoids the entire batch from being redelivered if your consumer throws an error during batch processing. This can be particularly useful when you are calling external APIs, writing messages to a database, or otherwise performing non-idempotent actions on individual messages within a batch.
- publish_date: '2023-03-01'
  title: Higher per-queue throughput
  description: The per-queue throughput limit has now been [raised to 400 messages
    per second](/queues/platform/limits/).
- publish_date: '2022-12-12'
  title: Increased per-account limits
  description: Queues now allows developers to create up to 100 queues per account,
    up from the initial beta limit of 10 per account. This limit will continue to
    increase over time.
- publish_date: '2022-12-13'
  title: sendBatch support
  description: The JavaScript API for Queue producers now includes a `sendBatch` method
    which supports sending up to 100 messages at a time.
```

## Examples

- [Stream Changelog](/stream/changelog/)
- [Pages Changelog](/pages/platform/changelog/)
