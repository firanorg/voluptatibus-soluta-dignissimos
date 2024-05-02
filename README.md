# @firanorg/voluptatibus-soluta-dignissimos [![Tweet](https://img.shields.io/twitter/url/http/shields.io.svg?style=social)](https://twitter.com/intent/tweet?text=%40sveltejs%20wrapper%20for%20i18next%20%0Ahttps%3A%2F%2Fgithub.com%2FNishuGoel%2F@firanorg/voluptatibus-soluta-dignissimos%0Avia%20%40TheNishuGoel%20%20&hashtags=i18next,sveltejs,svelte,javascript,webdev)

[![CI](https://github.com/firanorg/voluptatibus-soluta-dignissimos/workflows/CI/badge.svg)](https://github.com/firanorg/voluptatibus-soluta-dignissimos/actions?query=workflow%3ACI)
[![npm version](https://img.shields.io/npm/v/@firanorg/voluptatibus-soluta-dignissimos.svg)](https://www.npmjs.com/package/@firanorg/voluptatibus-soluta-dignissimos)
[![bundle size](https://img.shields.io/bundlephobia/minzip/@firanorg/voluptatibus-soluta-dignissimos?label=gzip%20bundle)](https://bundlephobia.com/package/@firanorg/voluptatibus-soluta-dignissimos)
[![License](http://img.shields.io/:license-mit-blue.svg)](https://github.com/firanorg/voluptatibus-soluta-dignissimos/blob/master/LICENSE)

Svelte wrapper for [i18next](https://i18next.com/)

```
npm i @firanorg/voluptatibus-soluta-dignissimos i18next
```

## Implementation

This library wraps an i18next instance in a Svelte Store to observe [i18next events](https://github.com/firanorg/voluptatibus-soluta-dignissimos/blob/main/src/translation-store.ts#L23)
so your Svelte components re-render e.g. when language is changed or a new resource is loaded by i18next.

## Quick Start

`i18n.js`:
```ts
import i18next from "i18next";
import { createI18nStore } from "@firanorg/voluptatibus-soluta-dignissimos";

i18next.init({
 lng: 'en',
 resources: {
    en: {
      translation: {
        "key": "hello world"
      }
    }
  },
  interpolation: {
    escapeValue: false, // not needed for svelte as it escapes by default
  }
});

const i18n = createI18nStore(i18next);
export default i18n;
```

`App.svelte`:
```svelte
<script>
  import i18n from './i18n.js';
</script>

<div>
    {$i18n.t('key')}
</div>
```

## Usage with Sveltekit

Sveltekit shares stores across requests on server-side. This means that one users request could change the language setting of another users rendering if that is still ongoing. To avoid this issue, use `setContext` to create request-scoped store instances:

`i18n.js`:
```ts
import i18next from "i18next";
import { createI18nStore } from "@firanorg/voluptatibus-soluta-dignissimos";

i18next.init({
 lng: 'en',
 resources: {
    en: {
      translation: {
        "key": "hello world"
      }
    }
  },
  interpolation: {
    escapeValue: false, // not needed for svelte as it escapes by default
  }
});

export default () => createI18nStore(i18next);
```

`routes/+layout.svelte`:
```sveltehtml
<script>
  import getI18nStore from "i18n.js";
  import { setContext } from "svelte";
  
  setContext('i18n', getI18nStore());
</script>
```

`routes/+page.svelte`:
```sveltehtml
<script>
  import { getContext } from "svelte";
  
  const i18n = getContext("i18n");
</script>

<div>
  <h1>{ $i18n.t("key") }</h1>
</div>
```

See full example project: [Svelte example](https://github.com/firanorg/voluptatibus-soluta-dignissimos/blob/main/example)

