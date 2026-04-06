# hugo-blog

A simple blog built with Hugo.

## Requirements

Verify Hugo installation:

```bash
sudo pacman -S hugo
hugo version
```

## Installation

Clone the repository:

```bash
git clone https://github.com/mutantsan/hugo-blog.git
cd hugo-blog
```

Initialize submodules (if the theme is included as a submodule):

```bash
git submodule update --init --recursive
```

## Development

Start the local development server:

```bash
hugo server -D
```

* Open http://localhost:1313 in your browser
* `-D` includes draft content

## Build

Generate static files:

```bash
hugo
```

The output will be in the `public/` directory.

## Deployment

Deploy the contents of the `public/` directory to your hosting provider.

Examples:

* GitHub Pages
* Netlify
* Vercel
