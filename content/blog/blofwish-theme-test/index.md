---
title: "Blowfish Theme Feature Test"
date: 2025-10-05
draft: false
tags: ["hugo", "blowfish", "theme", "testing"]
categories: ["Development"]
showComments: false
---

This post is dedicated to testing the **Blowfish theme capabilities** in Hugo.
It’s designed to cover formatting, special blocks, tabs, footnotes, and embeds.

---

## Callouts and Alerts

{{< alert >}}
**Warning!** This action is destructive!
{{< /alert >}}

{{< alert icon="fire" cardColor="#e63946" iconColor="#1d3557" textColor="#f1faee" >}}
This is an error!
{{< /alert >}}

## Carousel

{{< carousel images="gallery/*" interval="10000" >}}

## Code Importer

An example of importing code from an external source:

{{< codeimporter url="https://raw.githubusercontent.com/DataShades/ckanext-tables/refs/heads/master/ckanext/tables/data_sources.py" type="python" startLine="15" endLine="20" >}}

## Chart

{{< chart >}}
type: 'bar',
data: {
  labels: ['Tomato', 'Blueberry', 'Banana', 'Lime', 'Orange'],
  datasets: [{
    label: '# of votes',
    data: [12, 19, 3, 5, 3],
  }]
}
{{< /chart >}}
