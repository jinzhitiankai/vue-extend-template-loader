# 继承自Crysknight/vue-extend-template-loader；完善vue 模板继承的问题

# vue-extend-template-loader
A loader for extending vue sfc templates alongside simple extension

## How to connect

```
npm i -D vue-extend-template-loader
```

#### webpack.config.js - configureWebpack
```
module.exports = {
    configureWebpack: {
        module: {
            rules: [
                {
                    test: /\.vue$/,
                    enforce: 'pre',
                    loader: 'vue-extend-template-loader'
                }
            ]
        }
    }
};
```

#### webpack.config.js - chainWebpack
```
module.exports = {
    chainWebpack: config => {
        config.module
            .rule('extend-template')
            .test(/\.vue$/)
            .pre()
            .use()
            .loader('vue-extend-template-loader')
            .end()
            .end();
    }
};
```

## Supported operations (`mode="operationName"` in extender)
- append, prepend - appends or prepends markup to the queried node
- delete, remove - deletes node
- replace - replaces queried node with specified markup
- after, before - inserts markup before or after queried node

## Usage example:

### Base Component
```
<template>
    <div class="test-1">
        <div class="hello">Hello</div>
        <div class="hi">Hi</div>
    </div>
</template>

<script>
export default {
    name: 'Test1'
};
</script>
```

### Extender Component
*Don't forget to specify `type="extend"` in your template tag since without it loader will NOT process your template, 
and Vue will then completely replace the source template with it (breaking it ofc)*
```
<template type="extend">
    <extenders>
        <!-- Attribute query is similar to css selector -->
        <extender query=".test-1" mode="append">
            <div class="howdy">Howdy!</div>
        </extender>
        <extender query=".hi" mode="delete" />
    </extenders>
</template>

<script>
import Test1 from 'src/test_1';

export default {
    name: 'Test2',
    extends: Test1,
    computed: {
        dizzy() {
            return true;
        }
    }
};
</script>
```

### Result component
```
<template>
    <div class="test-1">
        <div class="hello">Hello</div>
        <div class="howdy">Howdy!</div>
    </div>
</template>

<script>
import Test1 from './test_1';

export default {
    name: 'Test2',
    extends: Test1,
    computed: {
        dizzy() {
            return true;
        }
    }
};
</script>
```
