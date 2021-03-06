# vuepress-theme-coding-api

> 📦 📝 🎨 A api-friendly theme for VuePress. Inspired by [zeit](https://zeit.co/docs). A fork of sqrtthree/vuepress-theme-api.

[Live demo](https://blog.sqrtthree.com/vuepress-theme-api/)

![image](https://user-images.githubusercontent.com/8622362/40341249-9b6e8b9e-5db6-11e8-97f5-41cadc87ce51.png)

## Built With

- [Node.js](https://nodejs.org/)
- [VuePress](https://github.com/vuejs/vuepress)

## Prerequisites

There are some global dependencies you need to set up.

- [Node.js](https://nodejs.org/)
- [VuePress](https://github.com/vuejs/vuepress)

## Getting Started

### Installing

```bash
# Install vuepress
yarn global add vuepress # OR npm install -g vuepress

# Install theme
yarn global add vuepress-theme-coding-api # OR npm install -g vuepress-theme-coding-api
```

### Configuration

Create VuePress config file `.vuepress/config.js` and provide a `theme` option.

```js
// .vuepress/config.js
module.exports = {
  title: 'Hello, World.',
  description: '📦 🎨 A api-friendly theme for VuePress.',
  theme: 'coding-api',
}
```

### As Easy as 1, 2, 3

```bash
# Create a markdown file and write something
echo '# Hello, World.' > Hello.md

# Start writing
vuepress dev .

# Build to static files
vuepress build .
```

## How to use?

Go to [docs](https://blog.sqrtthree.com/vuepress-theme-api/) to get more details.

## Starter kit

A out-of-the-box starter kit for RESTful API document is [here](https://github.com/sqrthree/vuepress-theme-api-starter-kit).

---

Thanks For:
> [sqrtthree.com](http://sqrtthree.com/) &nbsp;&middot;&nbsp;
> GitHub [@sqrthree](https://github.com/sqrthree) &nbsp;&middot;&nbsp;
> Twitter [@sqrtthree](https://twitter.com/sqrtthree)
