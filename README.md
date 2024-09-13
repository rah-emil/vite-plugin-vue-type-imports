<h2 align="center">vite-plugin-vue-type-imports</h2>

<p align="center">
  Enables you to import types and use them in your <code>defineProps</code> and <code>defineEmits</code>. Supports both Vue 2 and Vue 3.
</p>

<p align="center">
<a href="https://www.npmjs.com/package/vite-plugin-vue-type-imports" target="__blank"><img src="https://img.shields.io/npm/v/vite-plugin-vue-type-imports?color=a356fe&label=Version" alt="NPM version"></a>
</p>

> ⚠️ This Plugin is still in Development and there may be bugs. Use at your own risk.

## Install
```bash
# Install Plugin
npm i -D @rah-emil/vite-plugin-vue-type-imports
```

```ts
// vite.config.ts

import { defineConfig } from 'vite'
import Vue from '@vitejs/plugin-vue'
import VueTypeImports from '@rah-emil/vite-plugin-vue-type-imports'

export default defineConfig({
  plugins: [
    Vue(), 
    VueTypeImports(),
  ],
})
```

### Nuxt
```ts
// nuxt.config.ts

export default {
  buildModules: [
    '@rah-emil/vite-plugin-vue-type-imports/nuxt',
  ]
}
```

## Usage

```ts
// types.ts

export interface User {
  username: string
  password: string
  avatar?: string
}
```

```html
<script setup lang="ts">
import type { User } from '~/types'

defineProps<User>()
</script>

<template>...</template>
```

## Known limitations
- Namespace imports like `import * as Foo from 'foo'` are not supported.
- [These types](https://www.typescriptlang.org/docs/handbook/2/types-from-types.html) are not supported.
- The plugin currently only scans the content of `<script setup>`. Types defined in `<script>` will be ignored.

## Notes
- `Enum` types will be converted to Union Types (e.g. `type [name] = number | string`) , since Vue can't handle them right now.
- The plugin may be slow because it needs to read files and traverse the AST (using @babel/parser).

## Caveats
- **Do not reference the types themselves implicitly, it will cause infinite loop**.
Vue will also get wrong type definition even if you disable this plugin.

Illegal code:
```ts
export type Bar = Foo
export interface Foo {
  foo: Bar
}
```

Alternatively, you can reference the types themselves in their own definitions

Acceptable code:
```ts
export type Bar = string

export interface Foo {
  foo: Foo
  bar: Foo | Bar
}
```

- Ending the type name with something like `_1` and `_2` is not recommended, because it may **conflict** with the plugin's transformation result

These types may cause conflicts:
```ts
type Foo_1 = string
type Bar_2 = number
```

## License

[MIT License](https://github.com/jacobclevenger/vite-plugin-vue-gql/blob/main/LICENSE) © 2021-PRESENT [Jacob Clevenger](https://github.com/jacobclevenger)
