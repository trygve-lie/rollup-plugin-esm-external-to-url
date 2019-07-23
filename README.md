# rollup-plugin-esm-import-to-url

Rollup plugin to transform "bare" import specifiers to absolute URLs in ES modules

[![Dependencies](https://img.shields.io/david/trygve-lie/rollup-plugin-esm-import-to-url.svg?style=flat-square)](https://david-dm.org/trygve-lie/rollup-plugin-esm-import-to-url)
[![Build Status](http://img.shields.io/travis/trygve-lie/rollup-plugin-esm-import-to-url/master.svg?style=flat-square)](https://travis-ci.org/trygve-lie/rollup-plugin-esm-import-to-url)
[![Known Vulnerabilities](https://snyk.io/test/github/trygve-lie/rollup-plugin-esm-import-to-url/badge.svg?targetFile=package.json&style=flat-square)](https://snyk.io/test/github/trygve-lie/rollup-plugin-esm-import-to-url?targetFile=package.json) [![Greenkeeper badge](https://badges.greenkeeper.io/trygve-lie/rollup-plugin-esm-import-to-url.svg)](https://greenkeeper.io/)

## Installation

```bash
$ npm install rollup-plugin-esm-import-to-url
```

## Usage

```js
import esmImportToUrl from 'rollup-plugin-esm-import-to-url';

export default {
    input: 'source/main.js',
    plugins: [esmImportToUrl({
        external: {
            'some-library': 'http://cdn.com/some-library/v1',
        },
    })],
    output: {
        file: 'build.js',
        format: 'esm'
    }
};
```

## Description

This plugin transforms "bare" import specifiers to absolute URL specifiers in
ES  modules. The module refered too by the "bare" import specifier will be
treated  as an external and its source will not be included in the bundle but
refered to by the URL.

In our source:

```js
import { LitElement, html, css } from 'lit-element';
```

In our Rollup config, we map `lit-element` to a bundle on a CDN:

```js
export default {
    input: 'source/main.js',
    plugins: [esmImportToUrl({
        external: {
            'lit-element': 'https://cdn.pika.dev/lit-element/v2',
        },
    })],
    output: {
        file: 'build.js',
        format: 'esm'
    }
};
```

Our output bundle will then be:

```js
import { LitElement, html, css } from 'https://cdn.pika.dev/lit-element/v2';
```

## Options

This plugin take the following options:

| option             | default  | type      | required | details                                                                                                  |
| ------------------ | -------- | --------- | -------- | -------------------------------------------------------------------------------------------------------- |
| external           | `{}`     | `object`  | `false`  | Mapping between the "bare" import specifier (the object key) and the absolute URL (the object value).            |


## Note on externals

The imports defined for `external` to this module must not occure in the Rollup `external` option.
If so, this module will throw.

In other words, this will not work:

```js
export default {
    input: 'source/main.js',
    external: ['lit-element'],
    plugins: [esmImportToUrl({
        external: {
            'lit-element': 'https://cdn.pika.dev/lit-element/v2',
        },
    })],
    output: {
        file: 'build.js',
        format: 'esm'
    }
};
```

## ESM only

This module will only work when the output are ES modules.

## License

Copyright (c) 2019 Trygve Lie

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
