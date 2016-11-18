# babel-plugin-import-resolve-alias

Use aliases to resolve imports. Similar to webpack's [resolve-alias](https://webpack.github.io/docs/configuration.html#resolve-alias).

Inspired by [babel-plugin-module-resolver](https://github.com/tleunen/babel-plugin-module-resolver)

**!important!**
Affects only `import` statements, not `require` calls.

## Why does this matter?
Webpack has convenient feature [resolve-alias](https://webpack.github.io/docs/configuration.html#resolve-alias). You can avoid the double dots in your import declarations `animals/Cat` instead of `../../../animals/Cat`.

Using webpack for building packages is ok, but when it comes to running tests, you need some untrivial hacks to make this aliasing feature work. Why don't we just delegate it to Babel? So do we with this plugin.

You can just set up the aliases in the plugin setting in `.babelrc` and they'll be used by all the Babel invokers, let's say the builder and the test runner.

## Differences with webpack and babel-plugin-module-resolver
1. There is no `root` option, hence no multiple roots too. The root is always only one and it's the root of the project
2. Only `import` statements are handled. `require` calls remain unchanged.

## Usage

### Install

### Configure
``` json5 
{
  plugins: [
    ['import-resolve-alias', {
      alias: {
        'brilliant-lib': './app/lib', // the dot means path is relative to the project root
        'brilliant': './app/src',
        'one': 'another'
      }
    }]
  ]
}
```

### What to expect
Let us having the config from the previous section and project root is `/home/fred/proj/`.
- `import brilliant-lib/xyz` becomes `import /home/fred/proj/app/lib/xyz`
- `import brilliant-lib/xyz` becomes `import /home/fred/proj/app/lib/xyz`
- `import one` becomes `import another`

Generally [the table](https://webpack.github.io/docs/configuration.html#resolve-alias) from webpack documentation is the reference.
