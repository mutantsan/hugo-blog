---
title: "Blowfish Theme Feature Test"
date: 2025-10-05
draft: true
tags: ["hugo", "blowfish", "theme", "testing"]
categories: ["Development"]
---

This post is dedicated to testing the **Blowfish theme capabilities** in Hugo.
It’s designed to cover formatting, special blocks, tabs, footnotes, and embeds.

---

## Recent

Here are few recent posts:

{{< list limit=3 where="Type" value="blog" title="Recent posts" >}}

## Callouts and Alerts

{{< alert >}}
**Warning!** This action is destructive!
{{< /alert >}}

{{< alert icon="fire" cardColor="#e63946" iconColor="#1d3557" textColor="#f1faee" >}}
This is an error!
{{< /alert >}}

## Carousel

{{< carousel images="{gallery/*}" interval="0" >}}

## Code Importer

An example of importing code from an external source:

{{< codeimporter url="https://raw.githubusercontent.com/DataShades/ckanext-tables/refs/heads/master/ckanext/tables/data_sources.py" type="python" startLine="15" endLine="20" >}}

## Chart

{{< chart >}}
type: 'bar',
data: {
  labels: ['January', 'February', 'March', 'April', 'May', 'June', 'July'],
  datasets: [
    {
      type: 'line',
      label: 'Revenue',
      data: [12000, 15000, 14000, 17000, 16000, 18000, 20000],
      borderColor: 'rgba(75, 192, 192, 1)',
      backgroundColor: 'rgba(75, 192, 192, 0.2)',
      fill: true,
      tension: 0.4
    },
    {
      type: 'bar',
      label: 'Expenses',
      data: [8000, 9000, 8500, 10000, 9500, 11000, 11500],
      backgroundColor: [
        'rgba(255, 99, 132, 0.7)',
        'rgba(255, 159, 64, 0.7)',
        'rgba(255, 205, 86, 0.7)',
        'rgba(75, 192, 192, 0.7)',
        'rgba(54, 162, 235, 0.7)',
        'rgba(153, 102, 255, 0.7)',
        'rgba(201, 203, 207, 0.7)'
      ],
      borderColor: 'rgba(0,0,0,0.1)',
      borderWidth: 1
    },
    {
      type: 'line',
      label: 'Profit Margin %',
      data: [33, 40, 39, 41, 41, 39, 43],
      borderColor: 'rgba(255, 206, 86, 1)',
      backgroundColor: 'rgba(255, 206, 86, 0.2)',
      yAxisID: 'y1',
      tension: 0.3
    }
  ]
},
options: {
  responsive: true,
  interaction: {
    mode: 'index',
    intersect: false
  },
  stacked: false,
  plugins: {
    title: {
      display: true,
      text: 'Company Performance Overview'
    }
  },
  scales: {
    y: {
      type: 'linear',
      display: true,
      position: 'left',
      title: {
        display: true,
        text: 'USD'
      }
    },
    y1: {
      type: 'linear',
      display: true,
      position: 'right',
      title: {
        display: true,
        text: 'Profit Margin %'
      },
      grid: {
        drawOnChartArea: false
      }
    }
  }
}
{{< /chart >}}

## Mermaid

{{< mermaid >}}
flowchart TD
    A[Start Adventure] --> B{Choose Path}
    B --> |Forest| C[Find a Hidden Treasure]
    B --> |Mountain| D[Encounter a Dragon]
    B --> |River| E[Meet a Friendly Mermaid]
    C --> F[Return Home Rich]
    D --> F
    E --> F
    F --> G[Celebrate Your Journey]
{{< /mermaid >}}


## TypeIt

{{< typeit tag=h2 lifeLike=true loop=true >}}
Серьога, привіт. Це звернення для тебе.
Якщо ти це бачиш, то ти пес.
{{< /typeit >}}


## Youtube

{{< youtube id="dQw4w9WgXcQ" title="Never Gonna Give You Up" >}}
